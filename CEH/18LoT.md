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

