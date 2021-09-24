# Traceroute
Traceroute  tracks  the route packets taken from an IP network on their way to a given host. It utilizes the IP protocol's time to live (TTL) field and attempts to elicit an *ICMP TIME_EXCEEDED* response from each gateway along  the  path  to  the host.

`traceroute6` is equivalent to `traceroute -6`

`tcptraceroute` is equivalent to `traceroute -T`

If  the  probe answers come from different gateways, the address of each responding system will be printed.  If there is no response within a certain timeout, an `"*"` (asterisk) is printed for that probe.

In the modern network environment the traditional traceroute methods can not be always applicable, because of widespread use of firewalls. Such firewalls filter the "unlikely" UDP ports, or even ICMP echoes. To solve this, some additional tracerouting methods are implemented (including tcp), see [LIST OF AVAILABLE METHODS](https://github.com/sagarjain-js/Internship-work/blob/main/traceroute.md#list-of-available-methods) below. Such methods try to use particular protocol and source/destination port, in order to bypass firewalls.

### Help Menu
```
└─# traceroute --help
Usage:
  traceroute [ -46dFITnreAUDV ] [ -f first_ttl ] [ -g gate,... ] [ -i device ] [ -m max_ttl ] [ -N squeries ] [ -p port ] [ -t tos ] [ -l flow_label ] [ -w MAX,HERE,NEAR ] [ -q nqueries ] [ -s src_addr ] [ -z sendwait ] [ --fwmark=num ] host [ packetlen ]
Options:
  -4                          Use IPv4
  -6                          Use IPv6
  -d  --debug                 Enable socket level debugging
  -F  --dont-fragment         Do not fragment packets
  -f first_ttl  --first=first_ttl
                              Start from the first_ttl hop (instead from 1)
  -g gate,...  --gateway=gate,...
                              Route packets through the specified gateway
                              (maximum 8 for IPv4 and 127 for IPv6)
  -I  --icmp                  Use ICMP ECHO for tracerouting
  -T  --tcp                   Use TCP SYN for tracerouting (default port is 80)
  -i device  --interface=device
                              Specify a network interface to operate with
  -m max_ttl  --max-hops=max_ttl
                              Set the max number of hops (max TTL to be
                              reached). Default is 30
  -N squeries  --sim-queries=squeries
                              Set the number of probes to be tried
                              simultaneously (default is 16)
  -n                          Do not resolve IP addresses to their domain names
  -p port  --port=port        Set the destination port to use. It is either
                              initial udp port value for "default" method
                              (incremented by each probe, default is 33434), or
                              initial seq for "icmp" (incremented as well,
                              default from 1), or some constant destination
                              port for other methods (with default of 80 for
                              "tcp", 53 for "udp", etc.)
  -t tos  --tos=tos           Set the TOS (IPv4 type of service) or TC (IPv6
                              traffic class) value for outgoing packets
  -l flow_label  --flowlabel=flow_label
                              Use specified flow_label for IPv6 packets
  -w MAX,HERE,NEAR  --wait=MAX,HERE,NEAR
                              Wait for a probe no more than HERE (default 3)
                              times longer than a response from the same hop,
                              or no more than NEAR (default 10) times than some
                              next hop, or MAX (default 5.0) seconds (float
                              point values allowed too)
  -q nqueries  --queries=nqueries
                              Set the number of probes per each hop. Default is
                              3
  -r                          Bypass the normal routing and send directly to a
                              host on an attached network
  -s src_addr  --source=src_addr
                              Use source src_addr for outgoing packets
  -z sendwait  --sendwait=sendwait
                              Minimal time interval between probes (default 0).
                              If the value is more than 10, then it specifies a
                              number in milliseconds, else it is a number of
                              seconds (float point values allowed too)
  -e  --extensions            Show ICMP extensions (if present), including MPLS
  -A  --as-path-lookups       Perform AS path lookups in routing registries and
                              print results directly after the corresponding
                              addresses
  -M name  --module=name      Use specified module (either builtin or external)
                              for traceroute operations. Most methods have
                              their shortcuts (`-I' means `-M icmp' etc.)
  -O OPTS,...  --options=OPTS,...
                              Use module-specific option OPTS for the
                              traceroute module. Several OPTS allowed,
                              separated by comma. If OPTS is "help", print info
                              about available options
  --sport=num                 Use source port num for outgoing packets. Implies
                              `-N 1'
  --fwmark=num                Set firewall mark for outgoing packets
  -U  --udp                   Use UDP to particular port for tracerouting
                              (instead of increasing the port per each probe),
                              default port is 53
  -UL                         Use UDPLITE for tracerouting (default dest port
                              is 53)
  -D  --dccp                  Use DCCP Request for tracerouting (default port
                              is 33434)
  -P prot  --protocol=prot    Use raw packet of protocol prot for tracerouting
  --mtu                       Discover MTU along the path being traced. Implies
                              `-F -N 1'
  --back                      Guess the number of hops in the backward path and
                              print if it differs
  -V  --version               Print version info and exit
  --help                      Read this help and exit

Arguments:
+     host          The host to traceroute to
      packetlen     The full packet length (default is the length of an IP
                    header plus 40). Can be ignored or increased to a minimal
                    allowed value
```

### LIST OF AVAILABLE METHODS
In  general, a particular traceroute method may have to be chosen by -M name, but most of the methods have their simple cmdline switches (you can see them after the method name, if present).

- `default`
- `icmp -I`
  - `raw`    Use only raw sockets (the traditional way).
      This way is tried first by default (for compatibility reasons), then new dgram icmp sockets as fallback.
  - `dgram`  Use only dgram icmp sockets.
- `tcp -T`
  - `syn,ack,fin,rst,psh,urg,ece,cwr`
      Sets specified tcp flags for probe packet, in any combination.
  - `flags=num`
      Sets the flags field in the tcp header exactly to num.
  - `ecn`    
      Send syn packet with tcp flags ECE and CWR (for Explicit Congestion Notification, rfc3168).
  - `sack,timestamps,window_scaling`
      Use the corresponding tcp header option in the outgoing probe packet.
  - `sysctl` 
      Use current  sysctl `(/proc/sys/net/*)` setting for the tcp header options above and **ecn**.  Always set by default, if nothing else specified.
  - `mss=num`
      Use value of num for maxseg tcp header option (when syn).
  - `info`  
      Print tcp flags of final tcp replies when the target host is reached.
      **Default options is syn,sysctl.**
- `tcpconn`
- `udp -U`
- `udplite -UL`
  - `coverage=num`
      Set udplite send coverage to num.
- `dccp -D`
  - `service=num`
      Set DCCP service code to num (default is 1885957735).
- `raw   -P proto`
  - `protocol=proto`
      Use IP protocol proto (default 253).

#### Examples:
```
└─# traceroute google.com

traceroute to google.com (142.250.183.142), 30 hops max, 60 byte packets
 1  _gateway (192.168.43.1)  7.793 ms  7.684 ms  7.982 ms
 2  * * *
 3  56.8.81.253 (56.8.81.253)  70.292 ms 56.8.81.241 (56.8.81.241)  70.202 ms 56.8.81.245 (56.8.81.245)  88.502 ms
 4  172.26.101.40 (172.26.101.40)  87.097 ms  88.383 ms  88.337 ms
 5  172.26.101.51 (172.26.101.51)  87.003 ms 172.26.101.50 (172.26.101.50)  88.270 ms  88.223 ms
 6  172.25.111.0 (172.25.111.0)  88.104 ms  50.631 ms 172.25.111.6 (172.25.111.6)  50.421 ms
 7  172.25.111.1 (172.25.111.1)  60.607 ms 172.25.111.5 (172.25.111.5)  60.628 ms 172.25.111.1 (172.25.111.1)  60.513 ms
 8  172.16.92.145 (172.16.92.145)  86.417 ms 172.16.92.147 (172.16.92.147)  86.371 ms 172.26.40.5 (172.26.40.5)  77.717 ms
 9  172.26.40.5 (172.26.40.5)  86.217 ms  84.988 ms 172.16.4.152 (172.16.4.152)  99.823 ms
10  72.14.243.188 (72.14.243.188)  93.494 ms 74.125.51.166 (74.125.51.166)  99.667 ms 72.14.243.188 (72.14.243.188)  89.237 ms
11  * * *
12  142.251.77.100 (142.251.77.100)  79.586 ms 74.125.253.166 (74.125.253.166)  65.954 ms 142.251.69.102 (142.251.69.102)  58.720 ms
13  108.170.248.170 (108.170.248.170)  55.906 ms 142.250.214.113 (142.250.214.113)  64.386 ms 142.250.214.111 (142.250.214.111)  57.451 ms
14  108.170.248.193 (108.170.248.193)  55.380 ms bom07s31-in-f14.1e100.net (142.250.183.142)  49.965 ms  49.794 ms

```
Setting the traceroute Timeout Value
```
traceroute -w 7.0 google.com
```
Setting the Number of Tests
```
traceroute -q 1 google.com
```
Hiding Device Names
```
traceroute -n google.com
```
Setting the Initial TTL Value
```
traceroute -f 11 google.com
```

#### References:
[How to geek](https://www.howtogeek.com/657780/how-to-use-the-traceroute-command-on-linux/)
```
man traceroute
```
