# Module 12: Evading IDS, Firewalls, and Honeypots

## Perform Intrusion Detection

### Snort
  Should configure snort once.

  detect a variety of attacks and probes such as 
  - buffer overflows
  - stealth port scans
  - CGI attacks 
  - SMB probes
  - OS fingerprinting attempts

  ``` snort -W ```\
  list mac,IP,Ethernet Drivers
  
  ``` snort -dev -i 1 ```\
  enable Ethernet driver, start packet dump mode
  
  Snort.conf
  - Step #1: Set the network variables
  - HOME_NET (my IP)
  - EXTERNAL_NET any
  - DNS_SERVERS 8.8.8.8
  - SMTP_SERVERS, HTTP_SERVERS, SQL_SERVERS, TELNET_SERVERS, and SSH_SERVERS.
  - if you do not have any servers running on your machine, leave the line as it is. DO NOT make any changes in that line.
  - RULE_PATH, SO_RULE_PATH, PREPROC_RULE_PATH
  - WHITE_LIST_PATH, BLACK_LIST_PATH
  - Step #4: Configure dynamic loaded libraries
  - dynamicpreprocessor directory
  - Step #5: Configure preprocessors
  - Step #6: Configure output plugins
  - 
