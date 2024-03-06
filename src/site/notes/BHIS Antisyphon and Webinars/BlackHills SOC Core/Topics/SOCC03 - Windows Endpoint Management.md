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

A great place to research [[Windows\|Windows]] CLI commands is [SS64](https://ss64.com), 

### Begin with network connections

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/windows/netstat/#netstat" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### netstat
- *netstat* (or network statistics) is a cross-platform CLI tool to monitor network traffic
	- Unfortunately, some of the switches are different depending on the OS, and and the specifics can be found on the [Wikipedia Page](https://en.wikipedia.org/wiki/Netstat#Parameters)
	- It is mostly obsolete on [[Tool Deep-Dives/Linux/Linux\|Linux]], and was replaced with [[Tool Deep-Dives/Linux/ss\|ss (secure socket)]]. 
- It shows the live connections to the host system


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/windows/netstat/#important-windows-commands" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



## Important Windows Commands
1. `netstat`
	1. `-naob`
		2. `a` - displays all active TCP and connections and TCP/UDP ports
		3. `n` - displays active TCP connectives, expressed numerically, no attempt to determine name
		4. `o` - displays active TCP connections with their PID
		5. `b` - displays the executable involved
			1. **NOTE**: The executable is listed *after* all the connection it's made
			2. ![Day 2-4.png](/img/user/Attachments/Day%202-4.png)
			3. **NOTE**: On macOS, `b` reports the total bytes in the traffic
	2. `-f`
		1. *Windows specific*; shows FQDN, may need to be run multiple times
		2. Can be helpful in identifying services that don't resolve to a FQDN, and are suspicious
		3. However, also takes longer to run, so not necessarily helpful for broad traffic filtering





</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/windows/net/#net" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### NET
- `NET` is a [[Windows\|Windows]] command to manage and investigate network resources


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/windows/net/#commands" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



### Commands
1. `net view`
	1. Shows all shares active on the windows session
	2. You will need to run it against a clean system to know what's normal
2. `net use`
	1. Show who is making outbound connections from the system (*outbound connections)
3. `net session`
	1. Show who has an active session with this system right now (*inbound connections*)




</div></div>


### Follow up with Windows Processes

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/windows/tasklist/#tasklist" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### tasklist
- `tasklist` shows all tasks running on the computer, in this moment, and their PID
	- Can be run locally or remotely
- One problem is that all `svchosts.exe` appear identical, and can be easy to gloss over


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/windows/tasklist/#command" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



### Command
1. `tasklist`
	1. `tasklist /svc`
		1. For each exe running, it lists the associated services
	2. `tasklist /m`
		1. All the DLLs associated with each executable
	3. `tasklist /m /fi "pid eq [pid]`
		1. `/m` - Lists all tasks curring using the given exe/dll name
		2. `/fi` - Apply a pre-configured filter (e.g., by PID, but status, by username, etc.)
	4. `/u domain\user </p password>`
		1. Run `tasklist` as a different user
		2. If the password is not entered in the optional switch, then the user will be prompted after running




</div></div>


2. `wmic`
	1. `wmic process list full`
	2. `wmic process get name,parentprocessid,processid`
		1. shows the ID and process ID of each process running
	3. `wmic process where processid=[PID] get commandline`
		1. See if the file was launched via commandline,

### Lab: [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/BHIS-SOCC-lab-WindowsCLI\|WindowsCLI]]

### Lab: [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/BHIS-SOCC-lab-DeepBlueCLI\|DeepBlueCLI]]
