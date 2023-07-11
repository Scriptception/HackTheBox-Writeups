# Synced

- **Category**: Starting Point - Tier 0
- **Difficulty**: Very Easy
- **Tags**: Rsync, Network, Protocols, Reconnaisance, Anonymous/Guest Access
- **Tools**: nmap, rsync


# Walkthrough

- **TIP**: Using the man page for rsync (`man rsync`) will help you get a lot of the answers here

## Task 1: What is the default port for rsync?

> **Answer**: 873

## Task 2: How many TCP ports are open on the remote host?

```bash
> nmap -p- -sT --min-rate 5000 10.129.97.233
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-11 12:37 AEST
Nmap scan report for 10.129.97.233
Host is up (0.23s latency).
Not shown: 62746 closed tcp ports (conn-refused), 2788 filtered tcp ports (no-response)

PORT    STATE SERVICE
873/tcp open  rsync
```

> **Answer**: 1

## Task 3: What is the protocol version used by rsync on the remote machine?

```bash
> nmap -sV -p 873 10.129.97.233
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-11 12:39 AEST
Nmap scan report for 10.129.97.233
Host is up (0.23s latency).

PORT    STATE SERVICE VERSION
873/tcp open  rsync   (protocol version 31)
```

> **Answer**: 31

## Task 4: What is the most common command name on Linux to interact with rsync?

> **Answer**: rsync

## Task 5: What credentials do you have to pass to rsync in order to use anonymous authentication? anonymous:anonymous, anonymous, None, rsync:rsync

> **Answer**: None

## Task 6: What is the option to only list shares and files on rsync? (No need to include the leading -- characters)


> **Answer**: list-only

# Submit Root Flag

```bash
# List directories on the host
> rsync --list-only rsync://10.129.97.233
public          Anonymous Share

# List files in the directory
> rsync --list-only rsync://10.129.97.233/public
drwxr-xr-x          4,096 2022/10/25 09:02:23 .
-rw-r--r--             33 2022/10/25 08:32:03 flag.txt

# Download the flag
> rsync rysnc://10.129.97.233/public/flag.txt ./flag.txt

# Read the flag
> cat flag.txt
72eaf5344ebb84908ae543a719830519
```

> **Answer**: 72eaf5344ebb84908ae543a719830519