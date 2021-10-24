# Module 13: Hacking Web Servers

## Footprint

### Ghost Eye
  gathers information such as:
  - Whois lookup, DNS lookup, EtherApe, 
  - Nmap port scan, HTTP header grabber, 
  - Clickjacking test, Robots.txt scanner, 
  - Link grabber, IP location finder, and traceroute.

  1. Linux, in ghost_eye folder, ``` pip3 install -r requirements.txt ```
  2. Launch ``` python3 ghost_eye.py ```

### Skipfish
  1. start **WAMP Server** on windows 2016.
  2. ``` skipfish -o /root/test -S /usr/share/skipfish/dictionaries/complete.wl http://[IP Address of Windows Server 2016]:8080 ```\
    brute-force attack on the web server by using the complete.wl dictionary file\
    creates a directory named test in the root location
    
### httprecon 

### ID Serve

### Netcat and Telnet
  The primary security problems with Telnet:
  - It does not encrypt any data sent through the connection.
  - It lacks an authentication scheme.
  
  1. ``` nc -vv www.moviescope.com 80 ```\
    display the hosting information of the provided domain.
  2. ``` GET /HTTP/1.0 ```\
    perform the banner grabbing and gather information such as content type, last modified date, accept ranges, ETag, and server information.
  
  1. ``` telnet www.moviescope.com 80 ```
  2. ``` GET /HTTP/1.0 ```

### Nmap Scriptin Engine(NSE)
  http-enum.nse
  
  1. ``` nmap -sV --script=http-enum [target website] ```\
    Show PORT, STATE, SERVICE, VERSION
  2. ``` nmap --script hostmap-bfk -script-args hostmap-bfk.prefix=hostmap- [target website] ```\
  3. ``` nmap --script http-trace -d [target website] ```\
    This script will detect a vulnerable server that uses the TRACE method
  4. ``` nmap -p80 --script http-waf-detect [target website] ```\
    scan the host and attempt to determine whether a web server is being monitored by an IPS, IDS, or WAF.

### Uniscan 
  not only performs simple commands like ping, traceroute, and nslookup, but also does static, dynamic, and stress checks on a web server. 
  
  1. ``` uniscan -h ```
  2. ``` uniscan -u http://[target IP]:8080/CEH -q ```\
    -u switch is used to provide the target URL\
    -q switch is used to scan the directories
  3. ``` uniscan -u http://[target IP]:8080/CEH -we ```\
    use -w and -e together\
    -w enable file checks\
    -e enable robots.txt, sitemap.xml check
  4. ``` uniscan -u http://[target IP]:8080/CEH -d ```\
    -d Dynamic testing
  5. path: usr --> share --> uniscan --> report

## Perform Attack
  THC Hydra tool
  
  1. ``` nmap -p 21 [target IP] ```
    Check if FTP open
  2. ``` hydra -L [userList path] -P [password List path] ftp://[target IP] ```
