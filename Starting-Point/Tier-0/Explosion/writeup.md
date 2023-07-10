# Explosion

- **Category**: Starting Point - Tier 0
- **Difficulty**: Very Easy
- **Tags**: Network, Programming, RDP, Reconnaissance, Weak Credentials
- **Tools**: nmap, freerdp2-x11


# Walkthrough

## Task 1: What does the 3-letter acronym RDP stand for?

> **Answer**: Remote Desktop Protocol

## Task 2: What is a 3-letter acronym that refers to interaction with the host through a command line interface?

> **Answer**: CLI

## Task 3: What about graphical user interface interactions?

> **Answer**: GUI

## Task 4: What is the name of an old remote access tool that came without encryption by default and listens on TCP port 23?

> **Answer**: telnet

## Task 5: What is the name of the service running on port 3389 TCP?


```bash
> nmap -p 3389 -sV 10.129.1.13
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-10 19:06 AEST
Nmap scan report for 10.129.1.13
Host is up (0.29s latency).

PORT     STATE SERVICE       VERSION
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

> **Answer** ms-wbt-server

## Task 6: What is the switch used to specify the target host's IP address when using xfreerdp?

> **Answer**: /v: 

## Task 7: What username successfully returns a desktop projection to us with a blank password?

> **Answer**: Administrator

# Submit Root Flag

> **Answer**: 951fa96d7830c451b536be5a6be008a0
