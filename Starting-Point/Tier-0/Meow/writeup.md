# Meow

- **Category**: Starting Point - Tier 0
- **Difficulty**: Very Easy
- **Tags**: Misconfiguration, Network, Protocols, Reconnaissance, Telnet, Weak Credentials
- **Tools**: Nmap, Telnet


# Walkthrough


## Task 1: **What does the acronym VM stand for?**



> **Answer**: Virtual Machine

## Task 2: **What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell.**

> **Answer**: Terminal

## Task 3: **What service do we use to form our VPN connection into HTB labs?**

:heavy_check_mark: 

>  openvpn

## Task 4: **What is the abbreviated name for a 'tunnel interface' in the output of your VPN boot-up sequence output?**

> ✅ tun

## Task 5: **What tool do we use to test our connection to the target with an ICMP echo request?**

> ✅ ping

## Task 6: **What is the name of the most common tool for finding open ports on a target?**

> ✅ nmap

## Task 7: **What service do we identify on port 23/tcp during our scans?**

We can run a scan against the host to find open ports:

- `nmap -F --open 10.129.1.17`

This shows us that Telnet is open on the host (TCP/23):

```bash
Starting Nmap 7.80 ( https://nmap.org ) at 2023-06-30 22:12 AEST
Nmap scan report for 10.129.1.17
Host is up (0.24s latency).
Not shown: 99 closed ports
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 2.09 seconds
```

> ✅ telnet

## Task 8: **What username is able to log into the target over telnet with a blank password?**

We can attempt to login via telnet. When doing so, we are presented with a login prompt. ‘root’ is a known standard Linux user so we try that username first. Doing so shows that a password is not required and we are able to login as root with a blank password.

- `telnet 10.129.1.17`

> ✅ root

## Submit Flag

Flag is at `/root/flag.txt`

```
root@Meow:~# cat flag.txt 
b40abdfe23665f766f9c61ecba8a4c19
```

> ✅ **b40abdfe23665f766f9c61ecba8a4c19**
