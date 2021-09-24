# NetDiscover
netdiscover is an active/passive ARP reconnaissance tool, initially developed to gain information about wireless networks without DHCP servers in wardriving scenarios. It can also be used on switched networks. Built on top of libnet and libpcap, it can passively detect online hosts or search for them by sending ARP requests.

### Help Menu
```
└─# netdiscover --help 


netdiscover: invalid option -- '-'

Netdiscover 0.7 [Active/passive ARP reconnaissance tool]
Written by: Jaime Penalba <jpenalbae@gmail.com>

Usage: netdiscover [-i device] [-r range | -l file | -p] [-m file] [-F filter] [-s time] [-c count] [-n node] [-dfPLNS]
  -i device: your network device
  -r range: scan a given range instead of auto scan. 192.168.6.0/24,/16,/8
  -l file: scan the list of ranges contained into the given file
  -p passive mode: do not send anything, only sniff
  -m file: scan a list of known MACs and host names
  -F filter: customize pcap filter expression (default: "arp")
  -s time: time to sleep between each ARP request (milliseconds)
  -c count: number of times to send each ARP request (for nets with packet loss)
  -n node: last source IP octet used for scanning (from 2 to 253)
  -d ignore home config files for autoscan and fast mode
  -f enable fastmode scan, saves a lot of time, recommended for auto
  -P print results in a format suitable for parsing by another program and stop after active scan
  -L similar to -P but continue listening after the active scan is completed
  -N Do not print header. Only valid when -P or -L is enabled.
  -S enable sleep time suppression between each request (hardcore mode)

If -r, -l or -p are not enabled, netdiscover will scan for common LAN addresses.
```

### Examples:
Scan common LAN addresses on eth0:
```
netdiscover -i eth0
```
Fast scan common LAN addresses on eth0 (search only for gateways):
```
netdiscover -i eth0 -f
```
Scan some fixed ranges:
```
netdiscover -i eth0 -r 172.26.0.0/24
netdiscover -r 192.168.0.0/16
netdiscover -r 10.0.0.0/8
```
Scan common LAN addresses with sleep time 0.5 milliseconds instead of default 1:
```
netdiscover -s 0.5
```
Scan fixed range on fast mode with sleep time 0.5 milliseconds instead of default 1:
```
netdiscover -r 192.168.0.0/16 -f -s 0.5
```
Scan a range using 101 as last octet for SOURCE IP
```
netdiscover -r 10.1.0.0/16 -n 101
```
Only sniff for ARP traffic, don't send nothing:
```
netdiscover -p
```
Scan multiple Ranges from a File
```
netdiscover -l <file containing ranges>
```
  example ranges file
  ```
  cat ranges_file
  192.168.1.0/24
  10.0.2.0/24
  ```

#### References:
[kalilinuxtutorials](https://kalilinuxtutorials.com/netdiscover-scan-live-hosts-network/)
```
man netdiscover
```
