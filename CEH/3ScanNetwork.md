# Scan network

Nmap Network Scanning\
The Official Nmap Project Guide to Network Discovery and Security Scanning
	
https://nmap.org/book/toc.html

## Lab 1: Host Discovery

### nmap
``` -sn -PR [Target IP Address] ```
-sn: disables port scan and -PR: performs ARP ping scan.
		
``` nmap -sn -PU [Target IP Address] ```
-PU: performs the UDP ping scan.

``` nmap -sn -PE [Target IP Address] ```
-PE: performs the ICMP ECHO ping scan.

``` nmap -sn -PE 10.10.10.11-20 ```
ICMP ECHO ping sweep

``` nmap -sn -PP [target IP address] ```
ICMP timestamp ping scan

``` nmap -sn -PM [target IP address] ```
ICMP address mask ping scan

``` nmap -sn -PS [target IP address] ```
sends empty TCP SYN packets, ACK response mean alive

``` nmap -sn -PA [target IP address] ```
sends empty TCP ACK packets, RST response mean alive

``` nmap -sn -PO [target IP address] ```
sends different probe packets of different IP protocols to the target host. 

### Other tools
- Angry IP Scanner
- SolarWinds Engineer’s Toolset (https://www.solarwinds.com)
- NetScanTools Pro (https://www.netscantools.com)
- Colasoft Ping Tool (https://www.colasoft.com)
- Visual Ping Tester (http://www.pingtester.net)
- OpUtils (https://www.manageengine.com)

## Lab 2: Port and Service discovery

MegaPing, NetScanTools pro, Nmap, Hping3.

### nmap commands

``` nmap -sT -v [Target IP Address] ```
1. -sT: 
- performs the TCP connect/full open scan. 
- completes a three-way handshake with the target machine.
- displaying all the open TCP ports and services running on the target machine.
2. -v: enables the verbose output.

``` nmap -sS -v [Target IP Address] ```
1. -sS: stealth scan. TCP half-open scan.
2. The stealth scan involves resetting the TCP connection between the client and server abruptly before completion of three-way handshake signals, and hence leaving the connection half-open. This scanning technique can be used to bypass firewall rules, logging mechanisms, and hide under network traffic.

``` nmap -sX -v [Target IP Address] ```
1. -sX: perform Xmas scan.
2. Xmas scan sends a TCP frame to a target system with FIN, URG, and PUSH flags set. If the target has opened the port, then you will receive no response from the target system. If the target has closed the port, then you will receive a target system reply with an RST.

``` nmap -sM -v [Target IP Address] ```
1. -sM: perform TCP Maimon scan.
2. In the TCP Maimon scan, a FIN/ACK probe is sent to the target; if there is no response, then the port is Open|Filtered, but if the RST packet is sent as a response, then the port is closed.

``` nmap -sA -v [Target IP Address] ```
1. -sA: perform ACK flag probe scan.
2. The ACK flag probe scan sends an ACK probe packet with a random sequence number; no response implies that the port is filtered (stateful firewall is present), and an RST response means that the port is not filtered.

``` nmap -sU -v [Target IP Address] ```
1. -sU: perform UDP scan.

``` nmap -A [Target Subnet] ```
1. -A: enables aggressive scan. 
2. The aggressive scan option supports OS detection (-O), version scanning (-sV), script scanning (-sC), and traceroute (--traceroute). You should not use -A against target networks without permission.


### nmap Profile Editor：
- It is common with Nmap to want to run the same scan repeatedly. 
- On Zenmap, UI editor.

### hping

``` hping3 -A [Target IP Address] -p 80 -c 5 ```
- -A: ACK flag.
- -p: port number.
- -c: packet count.
- The ACK scan sends an ACK probe packet to the target host; no response means that the port is filtered. If an RST response returns, this means that the port is closed.

``` hping3 -8 0-100 -S [Target IP Address] -V ```
1. -8: scan mode.
- Mode
- default mode     TCP
- -0  --rawip      RAW IP mode
- -1  --icmp       ICMP mode
- -2  --udp        UDP mode
- -8  --scan       SCAN mode.
- Example: hping --scan 1-30,70-90 -S www.target.host
- -9  --listen     listen mode
- - 0-100: port numbers to scan
2. -S: set SYN flag
3. -V: verbose

## Lab 3: Perform OS Discovery

### Check TTL by wireshark

| Operating System (OS) | Time To Live | TCP Window Size  |
| ------------- |:-------------:| -----:|
| Linux (Kernel 2.4 and 2.6) | 64 | 5840 |
| Google Linux      | 64      |   5720 |
| FreeBSD | 64      |    65535 |
| OpenBSD | 64      |    16384 |
| Windows 95 | 32      |    8192 |

### Nmap Script Engine (NSE)

``` nmap -A [Target IP Address] ```
- -A: to perform an aggressive scan.
- The scan takes aprroximately 10 minutes to complete.

``` nmap -O [Target IP Address] ```
- -O: performs the OS discovery.

### unicornscan
``` unicornscan [Target IP Address] -Iv ```
- In this command, -I specifies an immediate mode and v specifies a verbose mode.

## Lab 4: Scan beyond IDS and Firewall

### Scan

``` nmap -f [Target IP Address] ```
- -f switch is used to split the IP packet into tiny fragment packets.
- Packet fragmentation refers to the splitting of a probe packet into several smaller packets (fragments) while sending it to a network. When these packets reach a host, IDSs and firewalls behind the host generally queue all of them and process them one by one. However, since this method of processing involves greater CPU consumption as well as network resources, the configuration of most of IDSs makes it skip fragmented packets during port scans.

``` nmap -g 80 [Target IP Address] ```
- In this command, you can use the -g or --source-port option to perform source port manipulation.
- Source port manipulation refers to manipulating actual port numbers with common port numbers to evade IDS/firewall: this is useful when the firewall is configured to allow packets from well-known ports like HTTP, DNS, FTP, etc.

``` nmap -mtu 8 [Target IP Address] ```
- In this command, -mtu: specifies the number of Maximum Transmission Unit (MTU) (here, 8 bytes of packets).
- Using MTU, smaller packets are transmitted instead of sending one complete packet at a time. This technique evades the filtering and detection mechanism enabled in the target machine.

``` nmap -D RND:10 [Target IP Address] ```
- -D: performs a decoy scan.
- RND: generates a random and non-reserved IP addresses.
- The IP address decoy technique refers to generating or manually specifying IP addresses of the decoys to evade IDS/firewall. This technique makes it difficult for the IDS/firewall to determine which IP address was actually scanning the network and which IP addresses were decoys. By using this command, Nmap automatically generates a random number of decoys for the scan and randomly positions the real IP address between the decoy IP addresses.

### Custom Packets by Colasoft Packet Builder

### Custom UDP/TCP Packets by Hping3

``` hping3 [Target IP Address] --udp --rand-source --data 500 ```
- --udp specifies sending the UDP packets to the target host.
- --rand-source enables the random source mode.
- --data specifies the packet body size.

``` hping3 -S [Target IP Address] -p 80 -c 5 ```
- -S specifies the TCP SYN request on the target machine.
- -p specifies assigning the port to send the traffic. dest port.
- -c is the count of the packets sent to the target machine.

``` hping3 [Target IP Address] --flood ```
- --flood: performs the TCP flooding.

### Custom Packets by Nmap

``` nmap [Target IP Address] --data 0xdeadbeef ```
- Nmap uses --data [hex string] (here, 0xdeadbeef) to send the binary data (o’s and 1’s) as payloads in the sent packets to scan beyond firewalls.

``` nmap [Target IP Address] --data-string "Ph34r my l33t skills" ```
- Nmap uses --data-string [string] (here, “Ph34r my l33t skills”) to send a regular string as payloads in the sent packets to the target machine for scanning beyond the firewall.

``` nmap --data-length 5 [Target IP Address] ```
- Nmap uses --data-length [len] (here, 5) to append the number of random data bytes to most of the packets sent without any protocol-specific payloads.

``` nmap --randomize-hosts [Target IP Address] ```
- Nmap uses --randomize-hosts to scan the number of hosts in the target network in random order to scan the intended target that is beyond the firewall.

``` nmap --badsum [Target IP Address] ```
- send the packets with bad or bogus TCP/UPD checksums to the intended target to avoid certain firewall rulesets.

### Other tools:
- NetScanTools Pro (https://www.netscantools.com).
- Ostinato (https://www.ostinato.org).
- WAN Killer (https://www.solarwinds.com).

## Lab 5: Draw Network Diagrams

- Network Topology Mapper
- OpManager (https://www.manageengine.com), 
- The Dude (https://mikrotik.com), 
- NetSurveyor (http://nutsaboutnets.com), 
- NetBrain (https://www.netbraintech.com), 
- Spiceworks Network Mapping Tool (https://www.spiceworks.com)

## Lab 6: Perform Network Scanning using Various Scanning Tools

### Metasploit

1. Connect to database

```
# service postgresql start
# msfconsole 
msf6 > db_status
	(msg: postgersql selected, no connection)
msf6 > exit
# msfdb
# service postgresql restart
# msfconsole 
msf6 > db_status
	(msg: Connected to msf)
```

2. nmap scan

``` msf6 > nmap -Pn -sS -A -oX Test 10.10.10.0/24 ```

3. import db

``` msf6 > db_import Test ```

4. check database

``` msf6 > hosts ```

5. list service

``` 
msf6 > db_services
(or msf6 > services)
```

6. list Metasploit port scan modules

```
msf6 > search portscan
msf6 > use auxiliary/scanner/portscan/syn
msf6 > info
	set INTERFACE eth0
	set PORTS 80
	set RHOSTS 10.10.10.5-20
	set THREADS 50
msf6 > run
```

7. detect OS
```
msf6 > back
msf6 > use auxiliary/scanner/smb/smb_version
```
- The Server Message Block (SMB) is a network protocol that enables users to communicate with remote computers and servers — to use their resources or share, open, and edit files. It’s also referred to as the server/client protocol, as the server has a resource that it can share with the client.

8. detect ftp version

``` msf6 >  use auxiliary/scanner/ftp/ftp_version ```

9. export scanned information to CSV file

``` msf6 > hosts -o Results.csv ```
