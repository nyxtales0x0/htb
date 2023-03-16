## Tasks

**What does the acronym VM stand for?**

=> Virtual Machine

**What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell.**

=> terminal

**What service do we use to form our VPN connection into HTB labs?**

=> openvpn

**What is the abbreviated name for a 'tunnel interface' in the output of your VPN boot-up sequence output?**

=> tun

**What tool do we use to test our connection to the target with an ICMP echo request?**

=> ping

**What is the name of the most common tool for finding open ports on a target?**

=> nmap

**What service do we identify on port 23/tcp during our scans?**

=> telnet

**What username is able to log into the target over telnet with a blank password?**

=> root

**Submit root flag**

=> b40abdfe23665f766f9c61ecba8a4c19

## Nmap scans

```
┌─[birb@parrot]─[~]
└──╼ $nmap 10.129.248.14
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 11:25 IST
Nmap scan report for 10.129.248.14
Host is up (0.41s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 53.92 seconds
```
