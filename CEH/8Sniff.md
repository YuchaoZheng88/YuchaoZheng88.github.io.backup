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
  
