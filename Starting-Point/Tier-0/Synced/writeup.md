# Synced

|            |                                                                  |
|------------|------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                          |
| Difficulty | Very Easy                                                        |
| OS         | Linux                                                            |
| Tags       | Rsync, Network, Protocols, Reconnaissance, Anonymous Access      |
| Tools      | `nmap`, `rsync`                                                  |

## Summary

An rsync daemon on its default port with anonymous read access to a `public`
share. The flag is sitting in the share root.

## Notes

- `man rsync` answers most of the guided questions on this box.

## Recon

Full TCP scan:

```bash
nmap -p- -sT --min-rate 5000 10.129.97.233
```

```
PORT    STATE SERVICE
873/tcp open  rsync
```

Version probe:

```bash
nmap -sV -p 873 10.129.97.233
```

```
PORT    STATE SERVICE VERSION
873/tcp open  rsync   (protocol version 31)
```

## Foothold

rsync allows anonymous module listing with `--list-only`. The daemon exposes
one share called `public`.

```bash
# List modules
rsync --list-only rsync://10.129.97.233
public          Anonymous Share

# List files in the module
rsync --list-only rsync://10.129.97.233/public
drwxr-xr-x          4,096 2022/10/25 09:02:23 .
-rw-r--r--             33 2022/10/25 08:32:03 flag.txt

# Pull the flag down
rsync rsync://10.129.97.233/public/flag.txt ./flag.txt
```

## Flag

```
72eaf5344ebb84908ae543a719830519
```

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | What is the default port for rsync? | 873 |
| 2 | How many TCP ports are open on the remote host? | 1 |
| 3 | What is the protocol version used by rsync on the remote machine? | 31 |
| 4 | What is the most common command name on Linux to interact with rsync? | rsync |
| 5 | What credentials do you have to pass to rsync in order to use anonymous authentication? | None |
| 6 | What is the option to only list shares and files on rsync? | list-only |
| Flag | Submit Root Flag | `72eaf5344ebb84908ae543a719830519` |
