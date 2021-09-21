# TFTP Enumeration
## \# What is TFTP?
TFTP \[Trivial File Transfer Protocol\] is a file sharing protocol which allows a client to get a file from or put a file onto a remote host. One of its primary uses is in the early stages of nodes booting from a local area network. TFTP has been used for this application because it is very simple to implement.

TFTP works in unauthenticated way which means a client can anonymously download and upload data (if upload functionality is provided by server administrator)

* TFTP runs on **UDP** protocol , By Default the port is **69** 

* *Default location of **TFTP** in Windows*
```
C:\ProgramData\WinAgents\TFTP Server 4\TFTPRoot
```
* *Default location of **TFTP** in Linux*
```
/var/lib/tftpboot/
```
## \# Enumeration
### \> Nmap script
```
nmap -sU -p 69 --script tftp-enum.nse --script-args tftp-enum.filelist=customlist.txt <host>
```
***tftp-enum.filelist*** - file name with list of filenames to enumerate at tftp server
> Good wordlist for tftp bruteforcing
[metasploit TFTP wordlist](https://github.com/rapid7/metasploit-framework/blob/master/data/wordlists/tftp.txt)

### \> Metasploit Bruteforcing

```
# msfconsole
```
```
msf6 > use auxiliary/scanner/tftp/tftpbrute
```
```
msf6 auxiliary(scanner/tftp/tftpbrute) > set RHOST <host>
```
The Default wordlist is [tftp.txt](https://github.com/rapid7/metasploit-framework/blob/master/data/wordlists/tftp.txt) but we can also use custom [backup](https://github.com/xajkep/wordlists/blob/master/discovery/backup_files_only.txt)   	   wordlists.

```
msf6 auxiliary(scanner/tftp/tftpbrute) > set DICTIONARY backup_files_only.txt
```
After setting up everything we can proceed to run the bruteforcing
```
msf6 auxiliary(scanner/tftp/tftpbrute) > run
```

### \> **TFTP** Client tool
* Installation
```
# apt install tftp
```

* Downloading and uploading files
```
# tftp <host> <port>
```
	
* To Download files we use ***get*** command
```
tftp> get <filename>
```
* To Upload files we use ***put*** command
```
tftp> put <filename>
```
> ***Note***
> If you are uploading binary you should first switch to **binary** mode with the following command
```
tftp> mode binary
```

### \> Possible Locations where tftp can be setuped
* Windows
`C:\`,`C:\inetpub\wwwroot`,`C:\ProgramData\WinAgents\TFTP Server 4\TFTPRoot`
* Linux
`/`,`/var/www/html/`,`/var/lib/tftpboot/`

### \> Reference
* [Wikipedia](https://en.wikipedia.org/wiki/Trivial_File_Transfer_Protocol)
* [Hacktricks](https://book.hacktricks.xyz/pentesting/69-udp-tftp)
* [Metasploit wordlist](https://github.com/rapid7/metasploit-framework/blob/master/data/wordlists/tftp.txt)
* [xajkep wordlist](https://github.com/xajkep/wordlists/blob/master/discovery/backup_files_only.txt)
