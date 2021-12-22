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

