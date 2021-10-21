# Module 12: Evading IDS, Firewalls, and Honeypots

## 1. Perform Intrusion Detection

### Snort
  Should configure snort once.

  detect a variety of attacks and probes such as 
  - buffer overflows
  - stealth port scans
  - CGI attacks 
  - SMB probes
  - OS fingerprinting attempts

  ``` snort -W ```\
  list mac,IP,Ethernet Drivers
  
  ``` snort -dev -i 1 ```\
  enable Ethernet driver, start packet dump mode
  
  #### Snort.conf
  - Step #1: Set the network variables
  - HOME_NET (my IP)
  - EXTERNAL_NET any
  - DNS_SERVERS 8.8.8.8
  - SMTP_SERVERS, HTTP_SERVERS, SQL_SERVERS, TELNET_SERVERS, and SSH_SERVERS.
  - if you do not have any servers running on your machine, leave the line as it is. DO NOT make any changes in that line.
  - RULE_PATH, SO_RULE_PATH, PREPROC_RULE_PATH
  - WHITE_LIST_PATH, BLACK_LIST_PATH
  - Step #4: Configure dynamic loaded libraries
  - dynamicpreprocessor directory
  - Step #5: Configure preprocessors
  - Step #6: Configure output plugins
  
  #### icmp-info.rules (in \Snort\rules)
  ``` alert icmp $EXTERNAL_NET any -> $HOME_NET 10.10.10.19 (msg:"ICMP-INFO PING"; icode:0; itype:8; reference:arachnids,135; reference:cve,1999-0265; classtype:bad-unknown; sid:472; rev:7;) ```\
  Add to the last line.
  
  #### Start Snort
  ``` snort -iX -A console -c C:\Snort\etc\snort.conf -l C:\Snort\log -K ascii ```\
  replace X with your device index number; in this: X is 1.\
  ``` snort -i1 -A console -c C:\Snort\etc\snort.conf -l C:\Snort\log -K ascii ```\
  After initializing interface and logged signatures, Snort starts and waits for an attack and triggers alerts when attacks occur on the machine.
  
  #### attack and check whether snort detects or not
  - ping the snort machine, can be detected as bad traffic
  
  #### check log
  \Snort\log\[ip]\ICMP_ECHO.ids
  
### ZoneAlarm FREE FIREWALL 2019
  
  The Add Zone window appears; choose the following:
  - Zone: Blocked
  - Hostname: www.moviescope.com
  - Description: Block This Site
  - Click Lookup; by doing this, we are blocking unwanted sites from browsing
  
  Other firewalls:
  - ManageEngine Firewall Analyzer (https://www.manageengine.com), 
  - pfSense (https://www.pfsense.org), 
  - Sophos XG Firewall (https://www.sophos.com), and 
  - Comodo Firewall (https://personalfirewall.comodo.com) 
  
### HoneyBOT
  if you want HoneyBOT to send you email alerts, check Send Email Alerts.\

## 2. Evade Firewalls using Various Evasion Techniques
  firewall bypassing techniques:
  - Port Scanning
  - Firewalking
  - Banner Grabbing
  - IP Address Spoofing
  - Source Routing
  - Tiny Fragments
  - Using an IP Address in Place of URL
  - Using Anonymous Website Surfing Sites
  - Using a Proxy Server
  - ICMP Tunneling
  - ACK Tunneling
  - HTTP Tunneling
  - SSH Tunneling
  - DNS Tunneling
  - Through External Systems
  - Through MITM Attack
  - Through Content
  - Through XSS Attack
 
 ### Nmap Evasion Techniques
  1. On Windows: firewall block Parrot Linux ip incoming traffic.
  1. On parrot linux: ``` nmap (target IP) ```, shows that all ports are filtered.
  1. ``` nmap -sS (target IP)```, perform TCP SYN Port Scan, still all filtered.
  1. ``` nmap -T4 -A (target IP) ```, INTENSE Scan, still filtered.\
    T4: Aggressive (4) speeds scans
    -A switch enables OS detection, version detection, script scanning, and traceroute.
  1. ``` nmap -sI (zombie IP) (target IP) ``` **Zombie Scan**, can get open ports on target.
  1. 
  
 
 
