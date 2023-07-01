# Fawn

- **Category**: Starting Point - Tier 0
- **Difficulty**: Very Easy
- **Tags**: FTP, Network, Protocols, Reconnaissance, Anonymous/Guest Access
- **Tools**: Nmap, Telnet


# Walkthrough

## Task 1: What does the 3-letter acronym FTP stand for?

> **Answer**: File Transfer Protocol

## Task 2: Which port does the FTP service listen on usually?

> **Answer**: 21

## Task 3: What acronym is used for the secure version of FTP?

> **Answer**: SFTP

## Task 4: What is the command we can use to send an ICMP echo request to test our connection to the target?

> **Answer**: ping

## Task 5: From your scans, what version is FTP running on the target?

We can use the `-sV` flag in `nmap` to probe for the service/version info.

- `nmap -sV -p 21 10.129.1.14`


```bash
Starting Nmap 7.80 ( https://nmap.org ) at 2023-07-01 15:37 AEST
Nmap scan report for 10.129.1.14
Host is up (0.23s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
Service Info: OS: Unix
```


> **Answer**: vsftpd 3.0.3

## Task 6: From your scans, what OS type is running on the target?

`nmap` has the ability to attempt to detect the Operating System of the target with the `-O` flag. We can provide the port with `-p 21` to save some time because we know that port is open.

**Note** that this requires root privileges

- `sudo nmap -O -p 21 10.129.1.14`

> **Answer**: Unix

## Task 7: 