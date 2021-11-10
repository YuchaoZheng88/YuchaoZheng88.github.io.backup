# Enumeration

## NetBIOS
  Network Basic Input Output System\
	NetBIOS port (137)
  
  ### Nbtstat (On Windows Server 2019)
  ``` nbtstat -a [IP address of the remote machine] ```
  - -a displays the NetBIOS name table of a remote computer.

  ``` nbtstat -c ```
  - -c lists the contents of the NetBIOS name cache of the remote computer.
  - It is possible to extract this information without creating a null session (an unauthenticated session).
  
  ``` net use ```
  - displays information about the target such as connection status, shared folder/drive and network information.

  ### NetBIOS Enumerator
  [website](http://nbtenum.sourceforge.net/)
  
  ### NSE Script Enumerator
  
  ``` nmap -sV -v --script nbstat.nse [Target IP Address] ```
  - -sV detects the service versions.
  - -v enables the verbose output (that is, includes all hosts and ports in the output)
  - --script nbtstat.nse performs the NetBIOS enumeration.
  
  ``` nmap -sU -p 137 -script nbstat.nse [Target IP Address] ```
  - -sU performs a UDP scan.
  - -p specifies the port to be scanned.
  - --script nbtstat.nse performs the NetBIOS enumeration.

  ### Other tools:
  - Global Network Inventory (http://www.magnetosoft.com)
  - Advanced IP Scanner (http://www.advanced-ip-scanner.com)
  - Hyena (https://www.systemtools.com)
  - Nsauditor Network Security Auditor (https://www.nsauditor.com)

## SNMP
  Simple Network Management Protocol
  - runs on UDP
  - SNMP uses port 161 by default

  two types of software components:
  - 1. SNMP agent. located on the networking device
  - 2. SNMP management station. communicates with the agent

  ### snmap-check
  
  ``` nmap -sU -p 161 [Target IP address] ```
  - check if target SNMP port is open
  - -sU performs a UDP scan.
  - -p specifies the port to be scanned.

  ``` snmp-check [Target IP Address] ```
  - enumerate SNMP
  - listing sensitive information such as:
  1. System information and User accounts
  2. Network information, Network interfaces
  3. Network IP and Routing information
  4. TCP connections and listening ports
  5. Processes, Storage information
  6. File system information, Device information, Share

  ### SoftPerfect Network Scanner
  [website](https://www.softperfect.com/products/networkscanner/)
  
  - Select scan property			
  - Scan results
  - view shared folders
  - ways to connect to the remote machine
  - If the selected host is not secure enough, you may use these options to connect to the remote machines. You may also be able to perform activities such as sending a message and shutting down a computer remotely. These features are applicable only if the selected machine has a poor security configuration.

  ### Other tools:
  - Network Performance Monitor (https://www.solarwinds.com)
  - OpUtils (https://www.manageengine.com)
  - PRTG Network Monitor (https://www.paessler.com)
  - Engineer’s Toolset (https://www.solarwinds.com)
  - WhatsUp® Gold (https://www.ipswitch.com)	

## LDAP
  LDAP (Lightweight Directory Access Protocol)
  - A client starts an LDAP session by connecting to a DSA (Directory System Agent), typically on TCP port 389, and sends an operation request to the DSA, which then responds.
  - BER (Basic Encoding Rules) is used to transmit information between the client and the server.
  
  ### Active Directory Explorer (AD Explorer)
  
  https://docs.microsoft.com/en-us/sysinternals/downloads/adexplorer
  
  ### Other tools:
  - Softerra LDAP Administrator (https://www.ldapadministrator.com)
  - LDAP Admin Tool (https://www.ldapsoft.com)
  - LDAP Account Manager (https://www.ldap-account-manager.org)
  - LDAP Search (https://securityxploded.com)
  - JXplorer (http://www.jxplorer.org)

## NFS
  NFS (Network File System) is a type of file system that enables computer users to access, view, store, and update files over a remote server. This remote data can be accessed by the client computer in the same way that it is accessed on the local system.
  
### RPCScan & SuperEnum
  
RPCScan communicates with RPC (remote procedure call) services and checks misconfigurations on NFS shares.

``` python3 rpc-scan.py [Target IP address] --rpc ```

SuperEnum
```
#cd SuperEnum
# echo "[target IP]" >> Target.txt
#./superenum
```
  
## DNS

### Perform DNS Enumeration using Zone Transfer

1. dig (Linux-based systems)
 
dig type: a, any, mx, ns, soa, hinfo, axfr, txt, ...	

``` dig ns [Target Domain] ```
- ns returns name servers in the result

``` 
dig @[[NameServer]] [[Target Domain]] axfr 
(dig ns1.bluehost.com www.certifiedhacker.com axfr)
```
- Can be used to test whether target DNS allows zone transfers or not.
- **AXFR** retrieves zone information
- DNS zone transfers using the AXFR protocol are the simplest mechanism to replicate DNS records across DNS servers.

2. nslookup(Windows-based systems)

```
c:\Users\Admin>nslookup
> set querytype=soa
> [domain] (like, certifiedhacker.com)
> ls -d ns1.bluehost.com
```
- set querytype=soa sets the query type to SOA (Start of Authority) record to retrieve administrative information about the DNS zone of the target domain certifiedhacker.com.
- ls -d requests a zone transfer of the specified name server. The result appears, displaying that the DNS server refused the zone transfer.

### Perform DNS Enumeration using DNSSEC Zone Walking

``` dnsrecon -h ```
``` dnsrecon -d www.certifiedhacker.com -z ```
- -d specifies the target domain 
- -z specifies that the DNSSEC zone walk be performed with standard enumeration
- in result: The "A" stands for "address" and this is the most fundamental type of DNS record: it indicates the IP address of a given domain. 

Using the DNSRecon tool, the attacker can enumerate general DNS records for a given domain (MX, SOA, NS, A, AAAA, SPF, and TXT). These DNS records contain digital signatures based on public-key cryptography to strengthen authentication in DNS.
- MX		Mail exchange record
- SOA		Start of [a zone of] authority record
- NS			Name server record
- A			Address record	
- AAAA		IPv6 address record
- SPF
- TXT		Text record

### Other tools
- LDNS (https://www.nlnetlabs.nl) 
- nsec3map (https://github.com)
- nsec3walker (https://dnscurve.org)
- DNSwalk (https://github.com) 

## RPC, SMB, FTP

### Perform RPC and SMB Enumeration using NetScanTools Pro

  NetScanTools Pro Demo, on Windows.
  1. https://www.netscantools.com/download.html
  2. RPC scan. 
  - Open RPC port: 111 Open NFS port: 2049
  3. SMB scan.

### Perform RPC, SMB, and FTP Enumeration using Nmap

  ``` nmap -p 21 [Target IP Address] ```
  - FTP services

  ``` nmap -T4 -A [Target IP Address] ```
  - may find out port 445 open

  ``` nmap -p [Target Port] -A [Target IP Address] ```
  - -p specifies the port to be scanned
  - -A specifies that the ACK flag is set

## Other

### Global Network Inventory

### Advanced IP Scanner

### Enum4linux
``` # enum4linux -h ```

``` # enum4linux -u martin -p apple -n [Target IP Address] ```
- -u user specifies the username to use
- -p pass specifies the password
- -n nmblookup
- nmblookup — NetBIOS over TCP/IP client used to lookup NetBIOS names

``` enum4linux -u martin -p apple -o [Target IP Address] ```
- -o: obtain the OS information.

``` num4linux -u martin -p apple -P [Target IP Address] ```
- -P: enumerate password policy.

``` enum4linux -u martin -p apple -G [Target IP Address] ```
- -G: enumerate group policy.

``` enum4linux -u martin -p apple -S [Target IP Address] ```
- -S: share policy.
