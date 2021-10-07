# 8. Sniff
  two types of sniffing: passive and active.
  - passive: on a hub-based network. (physical layer, all computer know information, no line mapping)
  - active: on a switch-based network. (data link layer, MAC address, only target get information)


## 1. Active Sniff
  techniques:
  - MAC Flooding: flooding the CAM table with fake MAC address and IP pairs until it is full
  - DNS Poisoning: tricking a DNS server into believing that it has received authentic information
  - ARP Poisoning: constructing a large number of forged ARP request and reply packets to overload a switch
  - DHCP Attacks: performing a DHCP starvation attack and a rogue DHCP server attack
  - Spoofing Attack: performing MAC spoofing, VLAN hopping, and STP attacks to steal sensitive information

### MAC Flooding
  > Force a switch to act as a hub, so can easily sniff the traffic.
  
  **macof**
  - a Unix and Linux tool.
  - of **dsniff** collection.
  - floods the switch`s CAM table, when MAC table fills up, the switch converts to a hub-like operation.
  
  ``` macof -i eth0 -d [Target IP Address] ```
  > -d: Specifies the destination IP address\
  > target a single system.
  
  ``` macof -i eth0 -n 10  ```
  > -i: specifies the interface\
  > -n: specifies the number of packets to be sent (here, 10).
  > start flooding the CAM table with random MAC addresses.
  > send packets with ramdom MAC and IP, to all devices in the local network.
  
### DHCP starvation
  > DHCP: automatically assign IP addresses.\
  > send DHCP requests all available IP addresses.\
  > valid users cannot renew their IP addresses.

  **Yersinia**
  - can take advantage of weaknesses in different network protocols, such as DHCP
  
  ``` yersinia -I ```
  - -I: Starts an interactive ncurses session.
  - press F2: select DHCP mode.
  - press X: list available attack opetions.
    - 0: sending RAW packet.
    - 1: sending DISCOVER packet.
    - 2: creating DHCP rogue server.
    - 3: sending RELEASE packet.
  - press 1: start a DHCP starvation attack.
  - Yersinia starts sending DHCP packets to the network adapter and all active machines in the LAN.
  - packets` destinations are FF:FF:FF:FF:FF:FF, Broadcast

### ARP Poisoning
  > ARP: find MAC addresses of a host from its IP addresses.\
  > when succeed: change IP of the attacker to the target.\
  > MITM attack.

  **arpspoof**
  
  ``` arpspoof -i eth0 -t 10.10.10.1 10.10.10.10 ```
  - 10.10.10.10 is IP address of the target system 10.10.10.1 is IP address of the access point or gateway.
  - tell 10.1 the gateway, I am 10.10
  - -i: specifies network interface.
  - -t: specifies target IP address.
  
  ``` arpspoof -i eth0 -t 10.10.10.10 10.10.10.1 ```
  - tell 10.10, I am gateway 10.1

  result
  - Wireshark will alert "duplicate use of 10.10.10.10 detected!"
  - on victim machine, you can see MAC address of IP(gateway, 10.1) and attacker are the same.
  
### MITM attack
  > prevention: HTTPS, VPN, SSH
  > how to: split client-server connection into 2: client-attacker, attacker-server
  
  **Cain & Abel**
  > attacker do
  - on windows
  - MAC Address Scanner -> All tests
  - ARP tab -> New ARP Poison Routing -> Select 2 hosts
  - Configuration / Routes Packets. Start capturing ARP packets.
  
  > victim do
  - on windows
  - cmd: ``` ftp [another host IP] ```
  - type username and password
  - The information can be found by attacker.
  
  Ftp: realy on clear-text, does not use encryption.
  
### Spoof a MAC address
  **TMAC**
  > Technitium MAC Address Changer
  > Random MAC Address -> Change now
  
  **SMAC**
  > another MAC address changer

## 2. Sniff using various tools
  
  **Wireshark**
  - [website](https://www.wireshark.org/)
  - uses [libpcap](http://www.tcpdump.org/)
  - winpcap on windows
  - captures live network traffic from 
    - Ethernet, IEEE 802.11, PPP/HDLC, 
    - ATM, Bluetooth, USB, Token Ring, 
    - Frame Relay, and FDDI networks.
     
  ``` http.request.method == POST ```
  > filter POST requests\
  > then, In String, Find "pwd".\
  > In real-time, when attackers gain the credentials of a victim’s machine, they attempt to capture its remote interface and monitor the traffic

  remote infterface of wireshark
  - https://documentation.help/Wireshark/ChCapInterfaceRemoteSection.html

  **Omnipeek Network Protocol Analyzer**
  
  **SteelCentral Packet Analyzer**
  - can analyze multi-gigabyte recordings.
  - [download](https://www.riverbed.com/au/forms/trial-downloads/appresponse11-trial.html)
  - Bandwidth Over Time

## 3. Detect Network Sniffing

### Detect ARP Poisoning
  **Xarp**
  - It detects critical network attacks that firewalls cannot cover.

### Detect promiscuous Mode
  using **Nmap** Scripting Engine(NSE)
  - ```  nmap --script=sniffer-detect [Target IP Address/ IP Address Range] ```
  - The result is "Likely in promiscuous mode"
  
  using **NetScanTools Pro**
  - In Promiscuous Mode Scanner, set taget IP/range.
  
