# LDAP(Lightweight Directory Access Protocol): Ports 389 and 636

LDAP, the Lightweight Directory Access Protocol, is a mature, flexible, and well supported standards-based mechanism for interacting with directory servers. It’s often used for authentication and storing information about users, groups, and applications, but an LDAP directory server is a fairly general-purpose data store and can be used in a wide variety of applications. 

# Tools to dump domain details from LDAP server:
## Nmap Scripts:
```
ldap-brute.nse
ldap-novell-getpass.nse
ldap-rootdse.nse
ldap-search.nse
```
##### Usage:
```
nmap -p 389 -v --script [ script ] [ IP/HOST ]
```
## ad-ldap-enum:

##### Requirements:

The package python-ldap is required for the script to execute. This can be installed with the following command:

```
apt-get install libsasl2-dev libldap2-dev libssl-dev
```
```
pip install python-ldap
```
```
git clone https://github.com/CroweCybersecurity/ad-ldap-enum.git
```

##### Usage:

```
# cd ad-ldap-enum
# python3 ad-ldap-enum.py [-h] -l LDAP_SERVER -d DOMAIN [-a ALT_DOMAIN] [-e] [-n] [-u USERNAME] [-p PASSWORD] [-v]

Active Directory LDAP Enumerator

optional arguments:
  -h, --help                                        show this help message and exit
  -v, --verbose                                     Display debugging information.
  -o FILENAME_PREPEND, --prepend FILENAME_PREPEND   Prepend a string to all output file names.
Server Parameters:
  -l LDAP_SERVER, --server LDAP_SERVER              IP address of the LDAP server.
  -d DOMAIN, --domain DOMAIN                        Authentication account's FQDN. If an alternative domain is not specified this will be also used as the Base DN for searching LDAP.
  -a ALT_DOMAIN, --alt-domain ALT_DOMAIN            Alternative FQDN to use as the Base DN for searching LDAP.
  -e, --nested                                      Expand nested groups.
Authentication Parameters:
  -n, --null                                        Use a null binding to authenticate to LDAP.
  -s, --secure                                      Connect to LDAP over SSL
  -u USERNAME, --username USERNAME                  Authentication account's username.
  -p PASSWORD, --password PASSWORD                  Authentication account's password.
```

##### Example:

```
python3 ad-ldap-enum.py -d contoso.com -l 10.0.0.1 -u Administrator -p P@ssw0rd
```

## ldapdomaindump:

If you have valid credentials to login into the LDAP server, you can dump all the information about the Domain Admin using:

##### Installation:
```
https://github.com/dirkjanm/ldapdomaindump.git
```
```
cd ldapdomaindump
```
```
pip install ldap3 dnspython future
```
```
python setup.py install
```
		OR
```
pip3 install ldapdomaindump 
```
###### With just the source:
```
python ldapdomaindump.py
```
###### After installing:
```
python -m ldapdomaindump
```
```
ldapdomaindump
```

##### Usage:

```
ldapdomaindump.py [-h] [-u USERNAME] [-p PASSWORD] [-at {NTLM,SIMPLE}] [-o DIRECTORY] [--no-html] [--no-json] [--no-grep] [--grouped-json] [-d DELIMITER] [-r] [-n DNS_SERVER] [-m] HOSTNAME
```
  
## ldapsearch:
Check null credentials or if your credentials are valid and Extracting everything from a domain:
```
ldapsearch -x -h <IP> -D '<DOMAIN>\<username>' -w '<password>' -b "DC=<1_SUBDOMAIN>,DC=<TDL>"
```
-x Simple Authentication

-h LDAP Server

-D My User

-w My password

-b Base site, all data from here will be given

Extract users:
```
ldapsearch -x -h <IP> -D 'MYDOM\john' -w 'johnpassw' -b "CN=Users,DC=mydom,DC=local"
```
Extract computers:
```
ldapsearch -x -h <IP> -D '<DOMAIN>\<username>' -w '<password>' -b "CN=Computers,DC=<1_SUBDOMAIN>,DC=<TDL>"
```
Extract my info:
```
ldapsearch -x -h <IP> -D '<DOMAIN>\<username>' -w '<password>' -b "CN=<MY NAME>,CN=Users,DC=<1_SUBDOMAIN>,DC=<TDL>"
```
Extract Domain Admins:
```
ldapsearch -x -h <IP> -D '<DOMAIN>\<username>' -w '<password>' -b "CN=Domain Admins,CN=Users,DC=<1_SUBDOMAIN>,DC=<TDL>"
```
Extract Domain Users:
```
ldapsearch -x -h <IP> -D '<DOMAIN>\<username>' -w '<password>' -b "CN=Domain Users,CN=Users,DC=<1_SUBDOMAIN>,DC=<TDL>"
```
Extract Enterprise Admins:
```
ldapsearch -x -h <IP> -D '<DOMAIN>\<username>' -w '<password>' -b "CN=Enterprise Admins,CN=Users,DC=<1_SUBDOMAIN>,DC=<TDL>"
```
Extract Administrators:
```
ldapsearch -x -h <IP> -D '<DOMAIN>\<username>' -w '<password>' -b "CN=Administrators,CN=Builtin,DC=<1_SUBDOMAIN>,DC=<TDL>"
```
Extract Remote Desktop Group:
```
ldapsearch -x -h <IP> -D '<DOMAIN>\<username>' -w '<password>' -b "CN=Remote Desktop Users,CN=Builtin,DC=<1_SUBDOMAIN>,DC=<TDL>"
```

#### Further more :
> [ad-ldap-enum tool](https://github.com/CroweCybersecurity/ad-ldap-enum)

> [ldapdomaindump](https://github.com/dirkjanm/ldapdomaindump)

> [HackTricks](https://book.hacktricks.xyz/pentesting/pentesting-ldap)

> [LDAP Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/LDAP%20Injection)
