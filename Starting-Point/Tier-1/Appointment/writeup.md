# Appointment

- **Category**: Starting Point - Tier 1
- **Difficulty**: Very Easy
- **Tags**: Web, Databases, Injection, Apache, MariaDB, PHP, SQL, Reconnaissance, SQL Injection
- **Tools**:


# Walkthrough

## Task 1: What does the acronym SQL stand for?

> **Answer**: Structured Query Language

## Task 2: What is one of the most common type of SQL vulnerabilities?

> **Answer**: SQL Injection

## Task 3: What is the 2021 OWASP Top 10 classification for this vulnerability?

[OWASP - A03:2021 – Injection](https://owasp.org/Top10/A03_2021-Injection/)

> **Answer**: A03:2021 - Injection

## Task 4: What does Nmap report as the service and version that are running on port 80 of the target?

Command: `nmap <IP> -p 80 -sV`

> **Answer**:Apache httpd 2.4.38 ((Debian)) 

## Task 5: What is the standard port used for the HTTPS protocol?

> **Answer**: 443

## Task 6: What is a folder called in web-application terminology?

> **Answer**: directory

## Task 7: What is the HTTP response code is given for 'Not Found' errors?

> **Answer**: 404

## Task 8: Gobuster is one tool used to brute force directories on a webserver. What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains?

> **Answer**: dir

## Task 9: What single character can be used to comment out the rest of a line in MySQL?

> **Answer**: #

## Task 10: If user input is not handled carefully, it could be interpreted as a comment. Use a comment to login as admin without knowing the password. What is the first word on the webpage returned?

Assuming the login credential check is done like so: `SELECT * FROM users WHERE username = '<username>' AND password = '<password>'`, we can use a comment to ignore the password check:
 - Username: `admin'#`
 - Passowrd: `anything`


> **Answer**: Congratulations

## Submit Root Flag

> **Answer**: e3d0796d002a446c0e622226f42e9672
