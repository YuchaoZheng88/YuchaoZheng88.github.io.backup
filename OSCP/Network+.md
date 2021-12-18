# Network+

## intro
- Ephemeral port, Non-ephemeral port.
- TCP port 80, is not conflit with, UDP port 80, on same machine.
## common ports
- Telnet: tcp/23, non-encrypted.
- SSH: tcp/22, encrypted.
- DNS: udp/53.
- SMTP: tcp/25, between mail servers, device outgoing mail.
- IMAP4(tcp/143), POP3(tcp/110): receive incoming email.
- SFTP: tcp/22, use SSH as underlying protocol, list/add/delete/etc
- FTP: tcp/20 (active data transfer) & tcp/21 (control), auth username and passwd
- TFTP: udp/69, trivial, very simple, no auth.
- DHCP: udp/67 & udp/68. DHCP reservation.
- SNMP: udp/161, gather metrics
- RDP: tcp/3389, Remote Desktop Protocol
- NTP: udp/123
- SIP: tcp/5060 & tcp/5061, voice over IP
- H.323: tcp/1720, voice over IP
- SMB: tcp/445(NetBIOS-less)
- LDAP: tcp/389. LDPS: tcp/636. 
## OSI Model
- All People Seem To Need Data Processing (Initail remember)
- Application, Presentation, Session, Transport, Network, Data link, Physical
## Ethernet
 Ethernet frame (layer 2 PDU)
 - |Preamble(7) | SFD(1) | Destination MAC(6) | Source MAC(6) | Type(v4 or v6)(2) | Payload(46-1500)(layer 3 and higher) | FCS |
 - (how many bytes of this part) above number meaning
 - MAC: 6 bytes = 48 bits, eg.  8c:2d:aa:4b:98:a7,  first 3 bytes: OUI, manufacturer, last 3 bytes: the serial number,
 
 Half-duplex
 - cannot send and receive simutaneously
 - LAN hubs are half-duplex
 - Switch can be configured as half-duple (usually when connect to one h-d device)
 - When collision happens, all devices wait random, send again.

Full-duplex
- send and received the same time

## Switch
- Spanning Tree Protocol, STP, prevent loop
- Mac address table: ||| MAC Address: Output Interface |||
- ARP: Address Resolution Protocol
- ``` arp -a ``` show local ARP table

## Broadcast Domain & Collision Domain
- broadcast stops at a router
- full-duplex switch to stop collision

## Unicast, Broadcast, Multicast
- broadcast is not used in IPv6, multicast instead

## PDU
- MTU: Maximum IP packet to transmit
- | DLC Header (14) | IP Header (20) | TCP Header (20) | TCP Data (1460) | FCS (4) |
- DLC Header: | Destination MAC(6) | Source MAC(6) | Type(v4 or v6)(2) |
- a tunneled traffic concern: tunnel may be smaller than your local Ethernet segment
- DF: Dont Fragment bit, in IP Header.
- if ping with DF.  | IP header (20) | ICMP Header (8) | data (1500-20-8=1472) |
- ``` ping -f -l 1472 8.8.8.8 ``` -f: not fragment. -l: length.

## Network Segmentation
- VLAN: One physical switch, seperate logically at layer 2.
- VLAN can be applied on different switches.
- A trunk can be used to connect different switches with many VLANs (max 4094), use only one cable. (802.1Q)
- VLAN frame is added to normal Ethernet frame.(12 bits, 4094 vlans, 0 and 4095 are reserved)

## Spanning Tree Protocol
- connet 2 switches with two cables, will create a loop
- IEEE standard 802.1D
- Rapid Spanning Tress Protocol: 802.1W

## Switch Interface Properties
- speed and duplex
- VLAN assignment
- Tag: Trunk port will tag outgoing traffic, untag incoming traffic.
- PoE: power on ethernet
- Port mirroring: eg. copy of traffic to IDS, which connected with switch.

## Static and Dynamic routing
- Static routing: Administrator define the routes.
- Dynamic routing: Routers send routes to each other, almost real-time. (dynamic routing protocol)
- Default routing

## IGP, EGP
- AS: Autonomous System
- <img width="450" alt="image" src="https://user-images.githubusercontent.com/91292763/146480977-2105f25d-b858-438f-afc0-59232bf2cb88.png">

## Dynamic Routing Protocols
- Distance-vector routing protocols.(count hop as distance, speed not account, good for small network)
- Link-state routing protocols.(Consider speed, scalable, in large networks, like OSPF)
- Hybird routing protocols. (like BGP)

## IPv4, v6 address
- IPv4: 32 bits = 4 bytes
- IPv6: 128 bits = 16 bytes 

compression IPv6:
- 2600:DDDD:1111:0001:0000:0000:0000:0001
- 2600:DDDD:1111: 1: 0: 0: 0: 1
- 2600:DDDD:1111:1::1

## configuring IPv6
- dual-stack routing: configure v4 and v6 same time, address, netmask, routing table, dynamic routing protocols specific for v4 or v6.

when sometimes some device do not support v6 but others do.
- 6to4 addressing: send v6 over v4 network. (IP protocol 41, Teredo, Miredo)
- 4in6 tunneling: tunnel v4 traffic on v6 network. 

NDP: Neighbor Discovery Protocol (replace v4 ARP)

SLAAC: Stateless Address Autoconfiguration.
- configure IP address without DHCP server.

DAD: Duplicate Address Detection.

Discover routers: 
- RS(Router Solicitation), ff02::2, broadcast to	all routers.
- RA(Router Advertisement), rounter send back its IP, also ff02::1 to all nodes.

Discover Neighbor:
- NS(Neighbor Solicitation), ask for a certain IPv6 address in network.
- NA(Neighbor Advertisement), send back its MAC address.

## Prioritizing Traffic
Packet shaping:
- control bandwidth usage.
- set important applications with higher priorities.

Qos: Quality of Service.
- Cos(Class of Service): OSI Layer2, Ethernet frame header.
- DiffServ: OSI Layer3, in routers or firewalls.
- DSCP(Differentiated Services Code Point): in IP header.

## NAT
Network Address Translation.
- RFC 1918 private IPv4 address. (class A, Bs, Cs)
- Router translate the private IP to its external IP.
- NAT Table: | PrivateIP:Private port | Public IP:Public port |

Port forwarding:
- Also called Static NAT or Destination NAT.

## ACL
- Allow or deny traffic.
- also used for NAT, QoS, etc.
- default rule is deny.

## Circuit Switching and Packet Switching

## SDN
- Software Defined Networking

## Binary Math

