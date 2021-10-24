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
  
