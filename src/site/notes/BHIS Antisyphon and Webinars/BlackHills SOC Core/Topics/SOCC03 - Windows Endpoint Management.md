---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/topics/socc-03-windows-endpoint-management/"}
---

## Windows endpoint management
> Point of trivia (though interesting and possibly useful)
> WoWw64 = Windows 32-bit on Windows 64-bit^[[WoW64 - Wikipedia](https://en.wikipedia.org/wiki/WoW64)], and is used to run 32-bit applications on 64-bit windows.
1. Windows and Live forensics
	1. These commands are the grunt-work required to verify your [[EDR\|EDR]] application works
		1. You **need** to be able to run these commands in terminal
		2. There are times your EDR tool will be lying to you, or the EDR is opaque in how it makes decisions
2. Begin with network connections
	1. Core Windows network commands

		2. `net view`
			1. Shows all shares active on the windows session
			2. You will need to run it against a clean system to know what's normal
		3. `net use`
			1. Show who is making outbound connections from the system (*outbound connections)
		4. `net session`
			1. Show who has an active session with this system right now (*inbound connections*)
3. Windows Processes
	1. `tasklist`
		1. Shows all tasks running in this moment and their PID
		2. Problem is all the `svchosts.exe` that are identical and don't give a shit
		3. `tasklist /svc`
			1. For each exe running, it lists the associated services
		4. `tasklist /m`
			1. All the DLLs associated with each executable
		5. `tasklist /m /fi "pid eq [pid]`
	2. `wmic`
		1. `wmic process list full`
		2. `wmic process get name,parentprocessid,processid`
			1. shows the ID and process ID of each process running
		3. `wmic process where processid=[PID] get commandline`
			1. See if the file was launched via commandline,

### Lab: [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/BHIS-SOCC-lab-WindowsCLI\|WindowsCLI]]

### Lab: [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/BHIS-SOCC-lab-DeepBlueCLI\|DeepBlueCLI]]
