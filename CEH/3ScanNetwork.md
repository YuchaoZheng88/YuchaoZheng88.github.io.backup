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

