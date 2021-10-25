# Hacking Web Applications

Methods:
- SQL injection attacks
- cross-site scripting (XSS)
- cross-site request forgeries (CSRF)
- insecure communications
- brute-force
- parameter tampering

## footprint
Tasks:
- Server Discovery
- Service Discovery
- Server Identification
- Hidden Content Discovery

### Web Application Reconnaissance
1. Whois lookup
  - Netcraft (https://www.netcraft.com)
  - SmartWhois (https://www.tamos.com)
  - WHOIS Lookup (http://whois.domaintools.com)
  - Batch IP Converter (http://www.sabsoft.com) 
2. DNS interrogation
  - Toolset (https://tools.dnsstuff.com)
  - DNSRecon (https://github.com)
  - DNS Records (https://network-tools.com)
  - Domain Dossier (https://centralops.net)

Steps:
1. ``` nmap -T4 -A -v [Target Web Application, domain name] ```
  - -T4: specifies setting time template (0-5)
  - -A: specifies setting ACK flag
  - -v: enables the verbose output
  - Get Information: target machine name, NetBIOS name, DNS name, MAC address, OS, and other information.
2. ``` telnet [target domain] 80 ```\
   ``` GET /HTTP/1.0 ```\
    displaying information related to the server name and its version, technology used
3. if the attacker entered an **IP address**, they receive the banner information of the target machine.\
   if they enter the **URL of a website**, they receive the banner information of the respective web server that hosts the website.

### WhatWeb
