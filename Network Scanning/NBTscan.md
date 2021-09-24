# NBTscan
NBTscan  is a program for scanning IP networks for NetBIOS name information. It sends NetBIOS status query to each address in supplied range and lists received information in human readable form. For each responded host it lists IP address, NetBIOS computer name, logged-in user name and MAC address (such as Ethernet).


**NBTscan produces a report like that:**
```
IP address       NetBIOS Name     Server    User             MAC address
------------------------------------------------------------------------------
192.168.1.2      MYCOMPUTER                 JDOE             00-a0-c9-12-34-56
192.168.1.5      WIN98COMP        <server>  RROE             00-a0-c9-78-90-00
192.168.1.123    DPTSERVER        <server>  ADMINISTRATOR    08-00-09-12-34-56
```
If run with `-v` switch NBTscan lists whole NetBIOS name table for each responded address. 

The output looks like that:
```
NetBIOS Name Table for Host 192.168.1.123:

Name             Service          Type
----------------------------------------
DPTSERVER        <00>             UNIQUE
DPTSERVER        <20>             UNIQUE
DEPARTMENT       <00>             GROUP
DEPARTMENT       <1c>             GROUP
DEPARTMENT       <1b>             UNIQUE
DEPARTMENT       <1e>             GROUP
DPTSERVER        <03>             UNIQUE
DEPARTMENT       <1d>             UNIQUE
??__MSBROWSE__?  <01>             GROUP
INet~Services    <1c>             GROUP
IS~DPTSERVER     <00>             UNIQUE
DPTSERVER        <01>             UNIQUE

Adapter address: 00-a0-c9-12-34-56
----------------------------------------
```

#### Help Menu
```
└─# nbtscan --help                                                                                                               2 ⨯
nbtscan: invalid option -- '-'

NBTscan version 1.6.
This is a free software and it comes with absolutely no warranty.
You can use, distribute and modify it under terms of GNU GPL 2+.

Usage:
nbtscan [-v] [-d] [-e] [-l] [-t timeout] [-b bandwidth] [-r] [-q] [-s separator] [-m retransmits] (-f filename)|(<scan_range>) 
	-v		verbose output. Print all names received
			from each host
	-d		dump packets. Print whole packet contents.
	-e		Format output in /etc/hosts format.
	-l		Format output in lmhosts format.
			Cannot be used with -v, -s or -h options.
	-t timeout	wait timeout milliseconds for response.
			Default 1000.
	-b bandwidth	Output throttling. Slow down output
			so that it uses no more that bandwidth bps.
			Useful on slow links, so that ougoing queries
			don't get dropped.
	-r		use local port 137 for scans. Win95 boxes
			respond to this only.
			You need to be root to use this option on Unix.
	-q		Suppress banners and error messages,
	-s separator	Script-friendly output. Don't print
			column and record headers, separate fields with separator.
	-h		Print human-readable names for services.
			Can only be used with -v option.
	-m retransmits	Number of retransmits. Default 0.
	-f filename	Take IP addresses to scan from file filename.
			-f - makes nbtscan take IP addresses from stdin.
	<scan_range>	what to scan. Can either be single IP
			like 192.168.1.1 or
			range of addresses in one of two forms: 
			xxx.xxx.xxx.xxx/xx or xxx.xxx.xxx.xxx-xxx.
```
#### EXAMPLES:

Scans the whole `C-class` network:
```
nbtscan -r 192.168.1.0/24
```
Scans a `range` from 192.168.1.25 to 192.168.1.137:
```
nbtscan 192.168.1.25-137
```
Scans C-class network. Prints results in `script-friendly format` using colon as field separator:
```
nbtscan -v -s : 192.168.1.0/24

output:

192.168.0.1:NT_SERVER:00U
192.168.0.1:MY_DOMAIN:00G
192.168.0.1:ADMINISTRATOR:03U
192.168.0.2:OTHER_BOX:00U
...
```
Scans IP addresses specified in `file iplist`:
```
nbtscan -f iplist
```
#### NETBIOS SUFFIXES
NetBIOS  Suffix, aka NetBIOS End Character (endchar), indicates service type for the registered name. The most known codes are listed below. `(U = Unique Name, G = Group Name)`
```
 Name                Number(h)  Type  Usage
 --------------------------------------------------------------------------

 <computername>         00       U    Workstation Service
 <computername>         01       U    Messenger Service
 <\--__MSBROWSE__>      01       G    Master Browser
 <computername>         03       U    Messenger Service
 <computername>         06       U    RAS Server Service
 <computername>         1F       U    NetDDE Service
 <computername>         20       U    File Server Service
 <computername>         21       U    RAS Client Service
 <computername>         22       U    Exchange Interchange(MSMail Connector)
 <computername>         23       U    Exchange Store
 <computername>         24       U    Exchange Directory
 <computername>         30       U    Modem Sharing Server Service
 <computername>         31       U    Modem Sharing Client Service
 <computername>         43       U    SMS Clients Remote Control
 <computername>         44       U    SMS Administrators Remote Control Tool
 <computername>         45       U    SMS Clients Remote Chat
 <computername>         46       U    SMS Clients Remote Transfer
 <computername>         87       U    Microsoft Exchange MTA
 <computername>         6A       U    Microsoft Exchange IMC
 <computername>         BE       U    Network Monitor Agent
 <computername>         BF       U    Network Monitor Application
 <username>             03       U    Messenger Service
 <domain>               00       G    Domain Name
 <domain>               1B       U    Domain Master Browser
 <domain>               1C       G    Domain Controllers
 <domain>               1D       U    Master Browser
 <domain>               1E       G    Browser Service Elections
 <INet~Services>        1C       G    IIS
 <IS~computer name>     00       U    IIS
```

#### References:
[nbtscan](https://github.com/resurrecting-open-source-projects/nbtscan)
```
man nbtscan
```
