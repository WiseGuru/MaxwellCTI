---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/wmi/","updated":"2024-03-11T11:07:20.000-07:00"}
---

> **NOTE**: CIM is the *open standard* of WMI, is cross-platform, and more efficient.
#### WMI and CIM
- *Common Information Model* (*CIM*) and *Windows Management Instrumentation* (*WMI*) are frameworks for managing Windows and other services
	- WMI is Windows Specific, and CIM is the open standard
	- As [[Tool Deep-Dives/Windows/Windows\|Windows]] moves towards a more open and [[Tool Deep-Dives/Linux/Linux\|Linux]] friendly, CIM has become the more preferred method of OS management
- WMI
	- Uses the older DCOM protocol for communication
		- Can be a little slower and less secure
	- Specific to Windows
	- Available on [[Tool Deep-Dives/Windows/PowerShell\|PowerShell]] 2.0 and later
- CMI
	- Uses WS-MAN (Web Services-Management) protocol
	- More cross-platform friendly
		- Can be used on non-Windows systems that support CIM
	- Generally faster
	- Available on [[Tool Deep-Dives/Windows/PowerShell\|PowerShell]] 3.0 and later
- For a current list of commands for WMI or CIM, use `Get-Command -Noun <*CIM*/*WMI*>`


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/windows/wmic/#wmic-commands" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



### WMIC Commands
1. `wmic process list full`
	1. List all processes
2. `wmic process get name,parentprocessid,processid`
	1. Shows the ID and process ID of each process running
3. `wmic process where processid=[PID] get commandline`
	1. See what commands were used to launch the process
		1. Processes started through mouse/keyboard interaction list the full path of the executable
		2. Processes started through command line *tend* to only show the name of the executable and any switches used, but it's anything the person enters into the CLI
		3. Check the end of the [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-WindowsCLI\|BHIS-SOCC-lab-WindowsCLI]] for details






</div></div>




# Metadata

### Sources
[Windows Management Instrumentation - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page)
[CIM | DMTF](https://www.dmtf.org/standards/cim)
[Should I use CIM or WMI with Windows PowerShell? - Scripting Blog \[archived\]](https://devblogs.microsoft.com/scripting/should-i-use-cim-or-wmi-with-windows-powershell/)
### Tags
#tools_win 