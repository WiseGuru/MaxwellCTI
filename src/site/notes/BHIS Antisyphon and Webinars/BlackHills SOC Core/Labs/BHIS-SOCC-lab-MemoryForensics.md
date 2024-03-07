---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/bhis-socc-lab-memory-forensics/"}
---


## LAB: Memory Forensics
[IntroLabs/IntroClassFiles/Tools/IntroClass/Memory/MemoryAnalysis.md at master · strandjs/IntroLabs · GitHub](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/Memory/MemoryAnalysis.md)

1. Running Volatility
	1. `windows.netscan`
		1. Checking active network connections
			1. ![Day 3-2.png](/img/user/Attachments/Day%203-2.png)
			2. `445`/SMB
				1. This is generally bad and cause for suspicion
				2. PID is 4
					1. `write that down in your copybook now`
			3. Connection is `CLOSED`
				1. Interesting because Microsoft will keep closed connections in a connection table
				2. PID is 5452, process name is TrustMe.exe
		2. Grepping `445`, `CLOSED`, `ESTABLISHED`, etc. can help to cut the noise
	2. `windows.pslist`
		1. Checking active processes
		2. cmd.exe
			1. users do not run CMD on a day-to-day basis, 
	3. `windows.pstree`
		1. Diving into one of the CMD processes, we find it tied back to `TrustMe.exe`
		2. ![Day 3-1.png](/img/user/Attachments/Day%203-1.png)
	4. `dllist --pid 5452`
		1. Shows which DLLs are associated with PID 5452
		2. We can see what files were loaded, when, and where from
		3. TrustMe.exe came from the Downloads folder...
	5. `windows.malfind.Malfind`
		1. The "easy button" to identify malware
