# Nmap

Nmap ("Network Mapper/Scanner") is an open source and free utility for network discovery and security auditing. It's a command-line utility that scans Network, Hosts, Ports, and also detects installed running applications. Nmap helps pentester to identify its targets, their available ports and services and search for vulnerabilities.The GUI (Graphical User Interface) for Nmap is Zenmap. It helps in network visualization for improved reporting, understanding and monitoring.

At the end I will share some of my tips to scan a network, hosts, ports.
### Lists of Commands 

		
    nmap --help 
    

		Nmap 7.91 ( https://nmap.org )
		Usage: nmap [Scan Type(s)] [Options] {target specification}
		TARGET SPECIFICATION:
		  Can pass hostnames, IP addresses, networks, etc.
		  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
		  -iL <inputfilename>: Input from list of hosts/networks
		  -iR <num hosts>: Choose random targets
		  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
		  --excludefile <exclude_file>: Exclude list from file
		HOST DISCOVERY:
		  -sL: List Scan - simply list targets to scan
		  -sn: Ping Scan - disable port scan
		  -Pn: Treat all hosts as online -- skip host discovery
		  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
		  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
		  -PO[protocol list]: IP Protocol Ping
		  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
		  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
		  --system-dns: Use OS's DNS resolver
		  --traceroute: Trace hop path to each host
		SCAN TECHNIQUES:
		  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
		  -sU: UDP Scan
		  -sN/sF/sX: TCP Null, FIN, and Xmas scans
		  --scanflags <flags>: Customize TCP scan flags
		  -sI <zombie host[:probeport]>: Idle scan
		  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
		  -sO: IP protocol scan
		  -b <FTP relay host>: FTP bounce scan
		PORT SPECIFICATION AND SCAN ORDER:
		  -p <port ranges>: Only scan specified ports
		    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
		  --exclude-ports <port ranges>: Exclude the specified ports from scanning
		  -F: Fast mode - Scan fewer ports than the default scan
		  -r: Scan ports consecutively - don't randomize
		  --top-ports <number>: Scan <number> most common ports
		  --port-ratio <ratio>: Scan ports more common than <ratio>
		SERVICE/VERSION DETECTION:
		  -sV: Probe open ports to determine service/version info
		  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
		  --version-light: Limit to most likely probes (intensity 2)
		  --version-all: Try every single probe (intensity 9)
		  --version-trace: Show detailed version scan activity (for debugging)
		SCRIPT SCAN:
		  -sC: equivalent to --script=default
		  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
		           directories, script-files or script-categories
		  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
		  --script-args-file=filename: provide NSE script args in a file
		  --script-trace: Show all data sent and received
		  --script-updatedb: Update the script database.
		  --script-help=<Lua scripts>: Show help about scripts.
		           <Lua scripts> is a comma-separated list of script-files or
		           script-categories.
		OS DETECTION:
		  -O: Enable OS detection
		  --osscan-limit: Limit OS detection to promising targets
		  --osscan-guess: Guess OS more aggressively
		TIMING AND PERFORMANCE:
		  Options which take <time> are in seconds, or append 'ms' (milliseconds),
		  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
		  -T<0-5>: Set timing template (higher is faster)
		  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
		  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
		  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
		      probe round trip time.
		  --max-retries <tries>: Caps number of port scan probe retransmissions.
		  --host-timeout <time>: Give up on target after this long
		  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
		  --min-rate <number>: Send packets no slower than <number> per second
		  --max-rate <number>: Send packets no faster than <number> per second
		FIREWALL/IDS EVASION AND SPOOFING:
		  -f; --mtu <val>: fragment packets (optionally w/given MTU)
		  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
		  -S <IP_Address>: Spoof source address
		  -e <iface>: Use specified interface
		  -g/--source-port <portnum>: Use given port number
		  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
		  --data <hex string>: Append a custom payload to sent packets
		  --data-string <string>: Append a custom ASCII string to sent packets
		  --data-length <num>: Append random data to sent packets
		  --ip-options <options>: Send packets with specified ip options
		  --ttl <val>: Set IP time-to-live field
		  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
		  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
		OUTPUT:
		  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
		     and Grepable format, respectively, to the given filename.
		  -oA <basename>: Output in the three major formats at once
		  -v: Increase verbosity level (use -vv or more for greater effect)
		  -d: Increase debugging level (use -dd or more for greater effect)
		  --reason: Display the reason a port is in a particular state
		  --open: Only show open (or possibly open) ports
		  --packet-trace: Show all packets sent and received
		  --iflist: Print host interfaces and routes (for debugging)
		  --append-output: Append to rather than clobber specified output files
		  --resume <filename>: Resume an aborted scan
		  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
		  --webxml: Reference stylesheet from Nmap.Org for more portable XML
		  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
		MISC:
		  -6: Enable IPv6 scanning
		  -A: Enable OS detection, version detection, script scanning, and traceroute
		  --datadir <dirname>: Specify custom Nmap data file location
		  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
		  --privileged: Assume that the user is fully privileged
		  --unprivileged: Assume the user lacks raw socket privileges
		  -V: Print version number
		  -h: Print this help summary page.
		EXAMPLES:
		  nmap -v -A scanme.nmap.org
		  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
		  nmap -v -iR 10000 -Pn -p 80
		SEE THE MAN PAGE (https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES


#### Network Scanning 
To deal with large no of hosts or a full network, we can attempt to scan targets using network sweeping techniques. The host discovery process consists of more than just sending ICMP packets. Nmap also sends a TCP ACk packet to port 80, a TCP SYN packet to port 443 and an ICMP timestamp to find out if the host is available or not.

    nmap -sn 10.10.10.1-254
    
For network scanning or multiple host scanning there are various techinques.

    nmap 10.10.10.1 10.10.10.2 10.10.10.3 
    nmap 10.10.10.* 
    nmap 10.10.10.1,2,3

#### Stealth / SYN Scanning
Involves TCP port scanning method, user requires raw sockets privileges, sends SYN packets to various ports on target without completing a TCP handshake. If the TCP port is open a SYN-ACK should be received from the target, informing us that the port is open. At this point the scanner does not send the final ACK to complete the three-way handshake.

    nmap -sS 10.10.10.1

#### Connect Scanning
User doesn't have raw socket privileges, TCP connect scan uses the `Berkeley sockets API` to perform the three-way handshake. It takes much longer time to complete than a SYN scan as nmap has to wait for the connection to complete before the API will return the status of the connection.

    nmap -sT 10.10.10.1

#### UDP Scanning 
Nmap uses a combination of two different methods to determine if the port is open or closed. for most of the ports, it will use the standard "ICMP port unreachable" method described earlier by sending an empty packet to a given port, However for common ports, such as 161 for SNMP service, it will send a `protocol-specific SNMP packet` in an attempt to get a response from an application bound to that port. To perform a UDP scan, the `-sU` switch is used and sudo is required to access raw sockets

    sudo nmap -sU 10.10.10.1

UDP scan (-sU) can also be used with a TCP SYN scan `(-sS)` to build a more complete picture of the target.

    sudo nmap -sS -sU 10.10.10.1
    
#### Port Scanning
To scan a single specific port `-p` switch is available.

    nmap -p 80 10.10.10.1
    
To scan all the 65536 ports `-p-` switch is available.

    nmap -p- 10.10.10.1
    
#### Top Ports Scan
To scan the top number of ports `--top-ports` switch is available.

    nmap --top-ports 20 10.10.10.1
    
#### Scanning from a File
A large number of IP addresses can be scanned by importing a file containing the list of IP addresses.

    nmap -iL /path/input-ips.txt
    
#### Nmap Scripting Engine (NSE)
We can use NSE to use user created scripts in order to automate various enumeration tasks such as DNS enumeration, Brute force attacks and even vulnerability identification. NSE scripts are located in the `/usr/share/nmap/scripts` directory.

    nmap 10.10.10.1 --script=smb-os-discovery
    nmap --script=dns-zone-transfer -p 53 10.10.10.1

To know more about any script use --script-help switch

    nmap --script-help smb-os-discovery
    
### Fast Scans

	## Nmap fast scan for the most 1000tcp ports used
	nmap -sV -sC -O -T4 -n -Pn -oA fastscan <IP> 
	
	## Nmap fast scan for all the ports
	nmap -sV -sC -O -T4 -n -Pn -p- -oA fullfastscan <IP> 
	
	## Nmap fast scan for all the ports slower to avoid failures due to -T4
	nmap -sV -sC -O -p- -n -Pn -oA fullscan <IP>

### Tips/ commands which I use personally:

    nmap 10.10.10.1/24
    nmap -vv -p- 10.10.10.1
    nmap -vv -sCV -A -O -p21,80,139,443,445 10.10.10.1
    nmap -vv -sCV -A -O -Pn -p21,80,139,443,445 10.10.10.1
    
    
    -vv = for very verbose mode
    -p- = for scanning all 65536 ports
    -sCV=  -sC is equivalent to --script=default and -sV is for enumerating versions
    -A  = for scanning in aggressive mode
    -O  = for identifying OS
    -p  = for scanning only specific ports
    -Pn = if ping probes are blocked on the target host.
