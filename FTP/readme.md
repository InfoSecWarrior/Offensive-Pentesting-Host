FTP Enumeration (21)
------------------------------------------------------------------------------------------------------------------
# What is Ftp?

FTP stand for File Transfer Protocol. It is a network protocol for transmitting files between computers over Transmission Control Protocol/Internet Protocol (TCP/IP) connections. Within the TCP/IP suite, FTP is considered an application layer protocol.


#### Detecting Open Port
	
     nmap -v -p 21 $ip

**Nmap Scripts**

       nmap –script=ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p 21 $ip

**Banner Grabbing**
	
        nc -nv $ip 21
 
#### Anonymous Authentication manually
````
      ftp $ip
````
#provide anonymous as username

#provide any passowrd
	
## FTP client commands

  
    Command 								    	          Description

	bye or close or quit						            Terminates an FTP connection.

	cd 							                            Changes the current working directory on the FTP host server.

	cwd									            Changes the current directory to the specified remote directory.

	dir							                            Requests a directory of files uploaded or available for download.
	
	get				                                                    Downloads a single file.

	ls										    Requests a list of file names uploaded or available for download.

	mget								            Interactively downloads multiple files.

	mput								            Interactively uploads multiple files.

	put								                    Uploads a single file.

	pwd									            Queries the current working directory.

	ren									            Renames or moves a file.
 
	site						                                   Executes a site-specific command.
	
	open									   Starts an FTP connection.

	pasv										    Tells the server to enter passive mode, in which the server waits for the client to establish a connection rather than attempting to connect to a port the client specifies.

#### Ftp User enumeration with github script

`````	
     https://raw.githubusercontent.com/pentestmonkey/ftp-user-enum/master/ftp-user-enum.pl
`````
         ftp-user-enum.pl -U /usr/share/seclists/Usernames/top-usernames-shortlist.txt -t $ip
`````	
     ftp-user-enum.pl -M iu -U /usr/share/seclists/Usernames/top-usernames-shortlist.txt -t $ip
`````
### Brute Forcing with hydra

	     hydra -l ftp -P password.txt ftp://$ip 
	
	     hydra -L username.txt -P password.txt ftp://$ip
	
## Metasploit Modules for FTP service

#### 1. auxiliary/scanner/ftp/anonymous
 #### 2. auxiliary/scanner/ftp/ftp_version
#### 3. auxiliary/scanner/ftp/ftp_login
#### 4. auxiliary/scanner/ftp/konica_ftp_traversal

	$  msfconsole
	
      1.Metasploit auxilliary for ftp anonymous check :

      msf6 > use auxiliary/scanner/ftp/anonymous

      msf6 auxiliary(scanner/ftp/anonymous) > set RHOSTS $ip

      msf6 auxiliary(scanner/ftp/anonymous) > run
      
  **Download All File from FTP**
    
        wget -m ftp://anonymous:anonymous@$ip
    
        wget -m --no-passive ftp://anonymous:anonymous@$ip
    
   **Uploading a binary or webshell**
      
         ftp> binary
         ftp> put file/name
      
   **Reference**
	
    	https://raw.githubusercontent.com/pentestmonkey/ftp-user-enum/master/ftp-user-enum.pl
