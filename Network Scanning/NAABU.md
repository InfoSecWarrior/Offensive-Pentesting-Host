# NAABU
Naabu is a port scanning tool written in Go that allows you to enumerate valid ports for hosts in a fast and reliable manner. It is a really simple tool that does fast SYN/CONNECT scans on the host/list of hosts and lists all ports that return a reply.

### Installation
#### From Binary

The installation is very easy-
[Releases](https://github.com/projectdiscovery/naabu/releases/)    

        - Download pre-built binary from the releases page according to your platform.
        - Extract them and move it to your $PATH and you are ready to go :
            tar -xvf naabu-linux-amd64.tar
            cp naabu-linux-amd64 /usr/local/bin/naabu
            naabu -version

#### From Source

naabu requires `libpcap` make sure to install library with `apt install -y libpcap-dev` on linux

    GO111MODULE=on go get -v github.com/projectdiscovery/naabu/v2/cmd/naabu 
    naabu -version

#### From Github

    git clone https://github.com/projectdiscovery/naabu.git; cd naabu/v2/cmd/naabu; go build; cp naabu /usr/local/bin/; naabu -version
    
#### From Docker
You can use the official dockerhub image at naabu. Simply run-

    docker pull projectdiscovery/naabu

The above command will pull the latest tagged release from the dockerhub repository.
After pulling / building the container using either way, run the following -

    docker run -it projectdiscovery/naabu -host hackerone.com > hackerone.com.txt

#### Windows
Windows version is currently not usable without docker.


## Usage
To run the tool on a target, just use the following command.

    naabu -host hackerone.com

This will run the tool against hackerone.com. There are a number of configuration options that you can pass along with this command. The verbose switch `-v` can be used to display verbose information.

naabu -host hackerone.com

                      __
      ___  ___  ___ _/ /  __ __
     / _ \/ _ \/ _ \/ _ \/ // /
    /_//_/\_,_/\_,_/_.__/\_,_/ v2.0.3

        projectdiscovery.io

    [WRN] Use with caution. You are responsible for your actions
    [WRN] Developers assume no liability and are not responsible for any misuse or damage.
    [INF] Running SYN scan with root privileges
    [INF] Found 4 ports on host hackerone.com (104.16.100.52)
    hackerone.com:80
    hackerone.com:443
    hackerone.com:8443
    hackerone.com:8080

The ports to scan for on the host can be specified via `-p` switch. It takes nmap format ports and runs enumeration on them.

    naabu -p 80,443,21-23 -host hackerone.com

By default, the Naabu checks for nmap's Top 100 ports. It supports following in-built port lists -


    -top-ports 100    => Scans for nmap top 100 port
    -top-ports 1000   => Scans for nmap top 1000 port
    -p -              => Scans for all ports from 1-65535.

You can also specify specific ports which you would like to exclude from the scan.

    naabu -p - -exclude-ports 80,443

The `o` flag can be used to specify an output file.

    naabu -host hackerone.com -o output.txt

To run the naabu on a list of hosts, `-iL` option can be used.

    naabu -iL hosts.txt

You can also get output in json format using `-json` switch. This switch saves the output in the JSON lines format.

    naabu -host hackerone.com -json

    {"host":"hackerone.com","ip":"104.16.99.52","port":8443}
    {"host":"hackerone.com","ip":"104.16.99.52","port":80}
    {"host":"hackerone.com","ip":"104.16.99.52","port":443}
    {"host":"hackerone.com","ip":"104.16.99.52","port":8080}

The ports discovered can be piped to other tools too. For example, you can pipe the ports discovered by naabu to [httpx](https://github.com/projectdiscovery/httpx) which will then find running http servers on the host.

    echo hackerone.com | naabu -silent | httpx -silent

    http://hackerone.com:8443
    http://hackerone.com:443
    http://hackerone.com:8080
    http://hackerone.com:80

If you want a second layer validation of the ports found, you can instruct the tool to make a TCP connection for every port and verify if the connection succeeded. This method is very slow, but is really reliable. This is similar to using nmap as a second layer validation

    naabu -host hackerone.com -verify

The speed can be controlled by changing the value of `rate` flag that represent the number of packets per second. Increasing it while processing hosts may lead to increased false-positive rates. So it is recommended to keep it to a reasonable amount

###Configuration file

Naabu support for config file, it allows each and every flag to define in config file, so you don't have to write them everytime, it's optional and not used on default run, default location of config file is `$HOME/.config/naabu/naabu.conf`, custom config file can be provided using `config` flag.


#### ==> Example Config File 

    # Number of retries
    # retries: 1
    # Packets rate
    # rate: 100
    # Timeout is the seconds to wait for ports to respond
    # timeout: 5
    # Hosts are the host to find ports for
    # host:
    #   - 10.10.10.10
    # Ports is the ports to use for enumeration
    # ports:
    #   - 80
    #   - 100
    # ExcludePorts is the list of ports to exclude from enumeration
    # exclude-ports:
    #   - 20
    #   - 30
    # Verify is used to check if the ports found were valid using CONNECT method
    # verify: false
    # Ips or cidr to be excluded from the scan
    # exclude-ips:
    #   - 1.1.1.1
    #   - 2.2.2.2
    # Top ports list
    # top-ports: 100
    # Attempts to run as root
    # privileged: true
    # Drop root privileges
    # unprivileged: true
    # Excludes ip of knows CDN ranges
    # exclude-cdn: true
    # SourceIP to use in TCP packets
    # source-ip: 10.10.10.10
    # Interface to use for TCP packets
    # interface: eth0
    # WarmUpTime between scan phases
    # warm-up-time: 2
    # nmap command to invoke after scanning invoke after scanning
    # nmap: nmap -sV

### Nmap integration

Naabu have integrated nmap support with `nmap` flag, in config file you can define any `nmap` command you wish to run on the result of naabu, make sure you have `nmap` installed to use this feature.

To make use of `nmap` flag, make sure to remove the comments from the config file at `$HOME/.config/naabu/naabu.conf`

Naabu also supports `nmap-cli` flag that let you run nmap commands directly on the results of naabu without making use of config file.

    echo hackerone.com | naabu -nmap-cli 'nmap -sV -oX naabu-output'
                      __       
      ___  ___  ___ _/ /  __ __
     / _ \/ _ \/ _ \/ _ \/ // /
    /_//_/\_,_/\_,_/_.__/\_,_/ v2.0.0        

        projectdiscovery.io

    [WRN] Use with caution. You are responsible for your actions
    [WRN] Developers assume no liability and are not responsible for any misuse or damage.
    [INF] Running TCP/ICMP/SYN scan with root privileges
    [INF] Found 4 ports on host hackerone.com (104.16.99.52)

    hackerone.com:443
    hackerone.com:80
    hackerone.com:8443
    hackerone.com:8080

    [INF] Running nmap command: nmap -sV -p 80,8443,8080,443 104.16.99.52

    Starting Nmap 7.01 ( https://nmap.org ) at 2020-09-23 05:02 UTC
    Nmap scan report for 104.16.99.52
    Host is up (0.0021s latency).
    PORT     STATE SERVICE       VERSION
    80/tcp   open  http          cloudflare
    443/tcp  open  ssl/https     cloudflare
    8080/tcp open  http-proxy    cloudflare
    8443/tcp open  ssl/https-alt cloudflare

### CDN Exclusion

Naabu also supports excluding CDN IPs being port scanned. If used, only `80` and `443` ports get scanned for those IPs. This feature can be enabled by using `exclude-cdn` flag.

Currently `cloudflare`, `akamai`, `incapsula` and `sucuri` IPs are supported for exclusions.

### [Download](https://github.com/projectdiscovery/naabu)
### Refrence: [projectdscovery](https://github.com/projectdiscovery/naabu)
