# Meow

|            |                                                                          |
|------------|--------------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                                  |
| Difficulty | Very Easy                                                                |
| OS         | Linux                                                                    |
| Tags       | Network, Protocols, Telnet, Reconnaissance, Misconfiguration, Weak Creds |
| Tools      | `nmap`, `telnet`                                                         |

## Summary

A Linux host with Telnet exposed on its default port. The `root` account has no
password set, so once the open port is found we can log straight in and read
the flag.

## Recon

Fast scan of the common ports:

```bash
nmap -F --open 10.129.1.17
```

```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-06-30 22:12 AEST
Nmap scan report for 10.129.1.17
Host is up (0.24s latency).
Not shown: 99 closed ports
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 2.09 seconds
```

Telnet on TCP/23 is the only thing of interest.

## Foothold

Connect to telnet and try common Linux account names. `root` is the obvious
first guess and the box accepts a blank password.

```bash
telnet 10.129.1.17
# login: root
# password: <blank>
```

## Flag

```
root@Meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19
```

## Guided Tasks

| #  | Question | Answer |
|----|----------|--------|
| 1  | What does the acronym VM stand for? | Virtual Machine |
| 2  | What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell. | Terminal |
| 3  | What service do we use to form our VPN connection into HTB labs? | openvpn |
| 4  | What is the abbreviated name for a 'tunnel interface' in the output of your VPN boot-up sequence output? | tun |
| 5  | What tool do we use to test our connection to the target with an ICMP echo request? | ping |
| 6  | What is the name of the most common tool for finding open ports on a target? | nmap |
| 7  | What service do we identify on port 23/tcp during our scans? | telnet |
| 8  | What username is able to log into the target over telnet with a blank password? | root |
| Flag | Submit Flag | `b40abdfe23665f766f9c61ecba8a4c19` |
