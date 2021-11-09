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
-sU: perform UDP scan.
