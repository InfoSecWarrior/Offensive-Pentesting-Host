# MySql Enumeration(3306)

## What is MySql?
MySql is a sql database . A relational database organizes data into one or more data tables in which data types may be related to each other; these relations help structure the data.

***Default MySql Port -> 3306***
### Default MySql Credentials
user | password
---- | --------
root | 
root | root

### Nmap Scripts
-> Information Gathering

```
nmap -sV -v -sT --script=mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 <host IP address> -p 3306
```

-> Username Enumeration
```
nmap --script=mysql-enum.nse –script-args userdb=<username wordlist> <host IP address>
```

### MySql Connect
-> Local Access
```
mysql -u <user> -p
Enter Password:- <password>
```

-> Remote Access
```
mysql -u <user> -h <host IP address> -p
Enter Password:- <password>
```

### Basic Commands
-> Printing Database
```
mysql> show databases;
```
-> Selecting a database
```
mysql> use <database>;
```
Example:-
```
mysql> use wordpress;
```
-> Printing tables
```
mysql> select * from <table>;
```
Example:-
```
mysql> select * from wp_users;
```
-> updating passwords
```
mysql> update <table> set <password column> = <hash method>('<password>') where ID = <ID number>;
```
Example:-
```
mysql> update wp_users set user_pass = MD5('123') where ID =1;
```

### MySql BruteForcing
-> Syntax
```
hydra -l <user> -P <pass list> -s <port> <host IP address> mysql
```
-> Example BruteForcing
```
hydra -l root -P /usr/share/wordlists/rockyou.txt -s 3306 10.10.11.10 mysql
```
### -> Reference
* [Wikipedia](https://en.wikipedia.org/wiki/MySQL)
* [Hacktricks](https://book.hacktricks.xyz/pentesting/pentesting-mysql)
