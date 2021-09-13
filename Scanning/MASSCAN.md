# MASSCAN
masscan  is  an  Internet-scale port scanner, useful for large scale surveys of the Internet, or of internal networks. While the default transmit rate is only 100 packets/second, it can optional go as fast as 25 million packets/second, a rate sufficient to scan the Internet in 3 minutes for one port.

#### Basic Commands:
    masscan --help

    MASSCAN is a fast port scanner. The primary input parameters are the IP addresses/ranges you want to scan, and the port numbers. An example is the following, which scans the 10.x.x.x network for web servers:
     masscan 10.0.0.0/8 -p80
    The program auto-detects network interface/adapter settings. If this fails, you'll have to set these manually. The following is an example of all the parameters that are needed:
     --adapter-ip 192.168.10.123
     --adapter-mac 00-11-22-33-44-55
     --router-mac 66-55-44-33-22-11
    Parameters can be set either via the command-line or config-file. The names are the same for both. Thus, the above adapter settings would appear as follows in a configuration file:
     adapter-ip = 192.168.10.123
     adapter-mac = 00-11-22-33-44-55
     router-mac = 66-55-44-33-22-11
    All single-dash parameters have a spelled out double-dash equivalent, so '-p80' is the same as '--ports 80' (or 'ports = 80' in the config file).
    To use the config file, type:
     masscan -c <filename>
    To generate a config-file from the current settings, use the --echo option. This stops the program from actually running, and just echoes the current configuration instead. This is a useful way to generate your first config file, or see a list of parameters you didn't know about. I suggest you try it now:
     masscan -p1234 --echo
     
#### Self Test
To find out if masscan is properly installed or not.

    masscan --regres
    
To test the performance. It will tell us the speed at which scan is being performed

    masscan 0.0.0.0/4 -p80 --rate 100 --offlinemasscan 0.0.0.0/4 -p80 --rate 1000 --offline
    masscan 0.0.0.0/4 -p80 --rate 100000 --offline
    
#### Network and Port scan

    masscan 10.10.10.1/24 -p21,80,443
    
Instead of giving subnet value we can also scan a range of IPs

    masscan 10.10.10.1-50 -p21,80,443
    
#### Scanning Subnets

    masscan -p80 10.10.10.0/24 --rate=1000 -e tun0 --router-ip 10.10.10.1

#### Banner Grabbing
To grab banners from IPs we scan, for this we may come across an issue. Since masscan uses a custom stack the OS may reject the packet. It uses asynchronous transmission & a custom TCP/IP stack. So different threads are used for transmission & reception of packets. So for now we need to specify a separate IP address in the same subnet.
Assume I have IP address 192.168.1.5, so we need to specify source IP in the 192.168.1.0/24 range

    masscan 10.10.10.1 -p 80,443 --banners --source-ip 192.168.1.150

#### Saving Results

    masscan 10.10.10.1/24 -p80,443 -oX output.xml
#### CONFIGURATION FILE FORMAT
The  configuration  file  uses the same parameter names as on the commandline, but without the -- prefix, and with an = sign between the name and the value. An example configuration file might be:
    # targets
    range = 10.0.0.0/8,192.168.0.0/16
    range = 172.16.0.0/14
    ports = 20-25,80,U:53
    ping = true

    # adapter
    adapter = eth0
    adapter-ip = 192.168.0.1
    router-mac = 66-55-44-33-22-11

    # other
    exclude-file = /etc/masscan/exludes.txt
           
By default, the program will read default configuration from the file /etc/masscan/masscan.conf


    --regress    = run a regression test, returns ´0´ on success and ´1´ on failure
    -p           = specifying ports to be scanned
    --offline    = don´t actually transmit packets
    --rate       = to determine the packet transmission rate.
    -e           = to specify raw network interface
    --router-ip  = to specify router's IP
    --banners    = specifies that banners should be grabbed
    -oX          = sets the output format to XML and saves the output in the given filename. This is equivalent to using the --output-format xml and --output-filename  parameters
