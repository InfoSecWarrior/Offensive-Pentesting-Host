# SMTP (25)

**What is smtp ?**
		
SMTP or Simple Mail Transfer Protocol is an application that is used to send, receive, and relay outgoing emails between senders and receivers.It is sometimes paired with IMAP or POP3, which handles the retrieval of messages, while SMTP primarily sends messages to a server for forwarding.
	
**Default port**		
		
    25, 465(ssl), 587(ssl)
	
 **Default File Location**
		
You can find the main configuration for Postfix Linux mail server in the /etc/postfix/main.cf file.
		
**SMTP Port Scanning** 
		
    nmap -v -p 25 $ip
	  
-v: Increase verbosity level (use -vv or more for greater effect)

-p : Only scan specified ports Ex: -p25; -p1-65535;
		
**Banner grabbing**
		
     nc -nv $ip 25
	
**Connection**

     telnet $ip 25

**Nmap Scripts**

	 # ls /usr/share/nmap/scripts/ | grep smtp
	 	smtp-brute.nse
		smtp-commands.nse
		smtp-enum-users.nse
		smtp-ntlm-info.nse
		smtp-open-relay.nse
		smtp-strangeport.nse
		smtp-vuln-cve2010-4344.nse
		smtp-vuln-cve2011-1720.nse
		smtp-vuln-cve2011-1764.nse
		
**Enumeration with nmap**		
		
      nmap -v -sC --script=smtp-enum-users.nse -p 25,465,587 $ip
	
-v : Increase verbosity level (use -vv or more for greater effect)

-sC : equivalent to --script=default

-p : Only scan specified ports
	
**User enumeration with smtp-user-enum command**

	git clone https://github.com/pentestmonkey/smtp-user-enum.git
   
   **Usage**
   
  	# smtp-user-enum --help
	
	Usage: smtp-user-enum [options] ( -u username | -U file-of-usernames ) ( -t host | -T file-of-targets )

     options are:

        -m n     Maximum number of processes (default: 5)
	
        -M mode  Method to use for username guessing EXPN, VRFY or RCPT (default: VRFY)
	
        -u user  Check if user exists on remote system
	
        -f addr  MAIL FROM email address.  Used only in "RCPT TO" mode (default: user@example.com)
	
        -D dom   Domain to append to supplied user list to make email addresses (Default: none)
                 Use this option when you want to guess valid email addresses instead of just usernames
                 e.g. "-D example.com" would guess foo@example.com, bar@example.com, etc.  Instead of 
                      simply the usernames foo and bar.
		      
        -U file  File of usernames to check via smtp service
	
        -t host  Server host running smtp service
	
        -T file  File of hostnames running the smtp service
	
        -p port  TCP port on which smtp service runs (default: 25)
	
        -d       Debugging output
	
        -w n     Wait a maximum of n seconds for reply (default: 5)
	
        -v       Verbose
	
        -h       This help message

   **Example** :-   
       
     ./smtp-user-enum.pl -M VRFY -U username.txt -t $ip
     ./smtp-user-enum.pl -M VRFY -u root -t $ip
     ./smtp-user-enum.pl -M VRFY -D metasploitable.localadmin -u root -t $ip
     
-M : Method to use for username guessing EXPN, VRFY or RCPT (default: VRFY)

-t : Server host running smtp service

-U : File of usernames to check via smtp service

-u : Check if user exists on remote system

-D : Domain to append to supplied user list to make email addresses (Default: none)
		
**Command to check VRFY user**
		
    VRFY root
		
**Command to ask EXPN mailing list**	
		
    EXPN root
	
**Command to check RCPT TO**		
		  
    RCPT TO:admin

**User Enumeration with Metasploit**	

Metasploit framework is pre-installed in klai linux. It is a automatic tool to help pentester.

   **msfconsole**
   
You can search different auxiliary and exploit modules by using the “search” operator. After identifying, you can use the “use” operator to run those modules against the target.
  
    use auxiliary/scanner/smtp/smtp_enum
    
    set RHOSTS $ip
    
    set RPORT 25
    
    set THREADS 10
    
    set USER_FILE /opt/username.txt
    
    run
		
In this metasploit module you can also add username list file where your smtp user with set USER_FILE filename command.
		
   **SMTP version Detection with metasploit**
   
     use auxiliary/scanner/smtp/smtp_version
     
     set RHOSTS $ip
     
     set RPORT 25
     
     set THREADS 10
     
     run
	
**Brute Force**			
		  
    hydra -l username -P Password.txt smtp://$ip
    
    hydra -L username.txt -p password smtp://$ip
    
    hydra -v -L username.txt -P password.txt smtp://$ip -t 4
    
-l :  LOGIN 

-L : FILE login with LOGIN name, or load several logins from FILE

-p :  PASS

-P : FILE try password PASS, or load several passwords from FILE

-t : run TASKS number of connected in parallel per target (default: 16)

-v :  verbose mode

### > Reference

*  [PentestMonkey](https://github.com/pentestmonkey/smtp-user-enum.git)
			
			
	
	
	
	

		
		
		
	
		
		
		
		








		
	
	

		
