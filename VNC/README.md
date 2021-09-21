# VNC (Virtual Network Computing) : 5900

It is a graphical desktop-sharing system that uses the Remote Frame Buffer protocol(RFB) to remotely control another computer. It runs on port number 5900 and web interface 5800. Server can connect to a viewer in "listening mode" on port 5500. It transmits the keyboard and mouse input from one computer to another, relaying the graphical screen updates, over a network.

## VNC password location
File that contain password for VNC Server
```
# ~/.vnc/passwd
```
## Enumeration Tools: 

### NMAP
Following command enumerates vnc service on a host.
```
nmap -sV -p <port> --script realvnc-auth-bypass,vnc-info,vnc-title <ip>
```
### Nmap Scripts

```
# ls /usr/share/nmap/scripts/ | grep vnc
      realvnc-auth-bypass.nse
      vnc-brute.nse
      vnc-info.nse
      vnc-title.nse
             
```

## Cracking Password
Following are the tools to Bruteforce user and password.

### Hydra

 ```
hydra -s <port> -L user.txt -P password.txt <ip> vnc
```
### Ncrack
```
ncrack -V -U user.txt  -P password.txt ip:port
```
### Medusa
```
medusa -h <ip> -U user.txt -P password.txt -M vnc
```
## Connecting to VNC Server
```
vncviewer <IP>:<PORT>
```
 ### vncconnect
 ```
 vncconnect [-display Xvnc-display] host[:port]
 ```
 ### vncserver
 ```
 vncserver -kill :<DISPLAY#>
 ```
## Metasploit scanner

### VNC_NONE_AUTH
 This auxiliary scans a range of hosts for VNC server.
```
#use auxiliary/scanner/vnc/vnc_none_auth
#show options
#set RHOSTS 192.168.1.0/24
#set BRUTEFORCE_SPEED
#set THREADS 
#run
```
### VNC_LOGIN
This auxiliary scans an IP address or range and try to login with given password or wordlists.
```
#use auxiliary/scanner/vnc/vnc_login      
#show options
#set RHOSTS 192.168.1.0/24
#set THREADS 
#run

```


