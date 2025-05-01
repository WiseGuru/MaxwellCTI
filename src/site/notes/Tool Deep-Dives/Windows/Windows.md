---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/windows/","noteIcon":""}
---

#### Windows
- Is an OS that hardly needs an introduction.

### Interesting Data Locations
1. `%appdata%` and `%localappdata%`
	1. Profile-specific file locations where applications often store data that is not 
	2. `%appdata%` takes you to *AppData/Raming*, and `%localappdata%` takes you to *AppData/Local*
2. `C:\ProgramData`
3. `C:\Windows`

### Getting Better Memory Dumps
- Getting a live memory dump
	- [Task Manager Live Memory Dump - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/task-manager-live-dump#create-a-live-kernel-memory-dump-of-the-system-using-task-manager)
- Enabling complete memory dumps on crash
	1. Open *System Properties* (two methods listed below)
		1. Search the start menu for `sysdm.cpl` or use the keyboard shortcut Win+R
		2. Windows Settings>System>About>Advanced System Settings
	2. Select the *Advanced* tab
	3. Startup and Recovery>Settings
	4. Under "Write debugging information", select the dropdown menu and choose "Complete memory dump"
		1. You can also choose a different write location and uncheck "Overwrite any existing file" to keep crashdumps over time



# Metadata

### Sources

### Tags
#tools_win 