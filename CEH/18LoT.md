# 18: IoT and OT Hacking

## Footprint
  Information:
  - protocols for Iot: (MQTT, ModBus, ZigBee, BLE, 5G, IPv6LoWPAN, etc.)
  - banner of the target IoT device, FCC ID information, certification granted to the device

  **SCADA**: Supervisory Control and Data Acquisition. used in:
  - water treatment plants, 
  - nuclear power plants, 
  - HVAC systems: Heating, ventilation, and air conditioning.
  - electrical transmission systems, 
  - home heating systems

### Gather Information using Online Footprinting Tools
  
  - Whois domain lookup
  - advanced Google hacking
  - Shodan search engine

  footprinting on the MQTT protocol:
  1. https://www.whois.com/whois/  search "www.oasis-open.org"
  2. **Oasis** is an organization that has published the **MQTT v5.0 standard**, which represents a significant leap in the refinement and capability of the messaging protocol that already powers IoT.
  3. visit "https://www.exploit-db.com/google-hacking-database" and quick search "SCADA"
  4. google type: ``` "login" intitle:"scada login" ```
  5. Advanced Google hacking refers to the art of creating complex search engine queries by employing advanced Google operators to extract sensitive or hidden information about a target company from the Google search results.
  6. Login "https://account.shodan.io/login"
  7. shodan search "port:1883"
  8. Port 1883 is the default MQTT port; 1883 is defined by IANA as MQTT over TCP.
  9. Search for Modbus-enabled ICS/SCADA systems: port:502
  10. Search for SCADA systems using PLC name: "Schneider Electric"
  11. Search for SCADA systems using geolocation: SCADA Country:"US"

## Capture and Analyze IoT Device Traffic

### Wireshark

1. ``` sudo snap install mqtt-explorer ```
2. **MQTT Explorer** is a comprehensive MQTT client that provides a structured overview of your MQTT topics and simplifies working with devices/services on your broker. [link](https://github.com/thomasnordquist/MQTT-Explorer)
3. Host: "mqtt.eclipseprojects.io", Port: 1883. CONNECT.
4. In wireshark, "Apply a display filter" -> "mqtt"
5. Select "Connect Ack" packet.
6. The broker sends the Connect Ack packet on receiving a Connect command request from the client.
7. see its: **Transmission Control Protocol**, **MQ Telemetry Transport Protocol**, and **Header Flags** 
8. Select "Subscribe Ack" packet.
9. To receive a relevant message, a client sends a SUBSCRIBE message to an MQTT broker. 
10. The MQTT broker confirms subscription by sending an acknowledgment back to the client using a SUBACK message.
11. After establishing a successful connection with the MQTT broker, the MQTT client can publish messages.
12. A Publish Release (PUBREL) packet is the response to a Publish Received (PUBREC) packet.
13. The Publish Complete (PUBCOMP) packet is the response to a Publish Release (PUBREL) packet.
