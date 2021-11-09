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

## LDAP
## NFS
## DNS
## RPC, SMB, FTP
## Other
