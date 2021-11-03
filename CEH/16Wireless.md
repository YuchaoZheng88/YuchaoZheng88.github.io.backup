# 16: Hacking Wireless Networks

IEEE 802.11 standard

Wired Equivalent Privacy (WEP) security algorithm

## Wireless traffic analysis
Wi-Fi protocols are unique at Layer 2, and traffic over the air is not serialized, which makes it easy to sniff and analyze wireless packets. 

### Wireshark

Wireshark can read live data from Ethernet, Token-Ring, FDDI, serial (PPP and SLIP), and 802.11 wireless LAN.

In Wireshark, open "WEPcrack-01.cap".

Other tools:
- AirMagnet WiFi Analyzer PRO (https://www.netally.com)
- SteelCentral Packet Analyzer (https://www.riverbed.com)
- Omnipeek Network Protocol Analyzer (https://www.liveaction.com)
- CommView for Wi-Fi (https://www.tamos.com) 
- Capsa Portable Network Analyzer (https://www.colasoft.com)

## Perform Wireless Attacks

Types:
- Fragmentation attack
- MAC spoofing attack
- Disassociation attack
- Deauthentication attack
- Man-in-the-middle attack
- Wireless ARP poisoning attack
- Rogue access points
- Evil twin
- Wi-Jacking attack

###  Crack a WEP network using Aircrack-ng

``` aircrack-ng `[captured file path]` ```

### Crack a WPA2 Network using Aircrack-ng

WPA2 is an upgrade to WPA\
Cipher Block Chaining Message Authentication Code Protocol (CCMP)

``` aircrack-ng -a2 -b [Target BSSID] -w [word list path] '[captured file path]' ```
- -a is the technique used to crack the handshake, 2=WPA technique.
- -b refers to bssid; replace with the BSSID of the target router.
- -w stands for wordlist; provide the path to a wordlist.

## Other crack tools
- Elcomsoft Wireless Security Auditor (https://www.elcomsoft.com)
- Portable Penetrator (https://www.secpoint.com)
- WepCrackGui (https://sourceforge.net)
- Pyrit (https://github.com)
- WepAttack (http://wepattack.sourceforge.net)
