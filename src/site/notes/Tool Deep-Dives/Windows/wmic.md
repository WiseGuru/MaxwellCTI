---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/wmic/"}
---

#### WMIC
- *Windows Management Instrumentation Command-line (WMIC)* is a (now deprecated) command-line method of working with [[Tool Deep-Dives/Windows/WMI\|WMI]].
	- The new/supported method is using [[Tool Deep-Dives/Windows/PowerShell\|PowerShell]], and commands can be found with `Get-Command -Noun WMI*`

### WMIC Commands
1. `wmic process list full`
	1. List all processes
2. `wmic process get name,parentprocessid,processid`
	1. Shows the ID and process ID of each process running
3. `wmic process where processid=[PID] get commandline`
	1. See what commands were used to launch the process
		1. Processes started through mouse/keyboard interaction list the full path of the executable
		2. Processes started through command line *tend* to only show the name of the executable and any switches used, but it's anything the person enters into the CLI
		3. Check the end of the [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/BHIS-SOCC-lab-WindowsCLI\|BHIS-SOCC-lab-WindowsCLI]] for details





# Metadata

### Sources
[WMI command-line (WMIC) utility - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmic)
### Tags
#tools_win 