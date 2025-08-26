---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/power-shell/","updated":"2024-03-28T11:16:08.000-07:00"}
---


#### PowerShell
- *PowerShell* is the defacto [[Tool Deep-Dives/Windows/Windows\|Windows]] CLI management tool
	- While it's a little more cumbersome than other programming tools, it's much easier to learn and read by non-specialists.^[Though some common commands have aliases that match the shorter version.]
		- For example, `cd .\Tools` is impenetrable for someone who doesn't know how `cd` works, but `Set-Location -Path C:\Tools` literally spells it out for you.
- Commands are constructed with a `Verb-Noun` syntax
	- e.g., `Get-ADUser` to retrieve information for a user in [[Tool Deep-Dives/Windows/Active Directory\|Active Directory]] or `Set-ADUser` to modify that user
- If you're not sure which command you want to use, you can enter **`Get-Command`** to find all commands available.
	- `Get-Command -Noun <string>` will find all commands that have a particular phrase in the *Noun* part of the command
		- Use a `*` to indicate wildcards, and can be used more than once in the string.
		- e.g., `Get-Command -Noun WMI*` will find all commands for the [[Tool Deep-Dives/Windows/WMI\|WMI]]
		- e.g., `Get-Command -Verb Remove` will find all commands with the *Remove* verb
	- You can pipe to `Select-String` if you're note entirely sure what you're looking for, but it won't give you as much information^[Command type, version, source, etc.] as a correctly formatted command.
- If you don't want to type the whole command out, you can use "Tab" to autocomplete based on the available information.
	- Continuing to press tab will cycle through the available commands.
	- Example, `Get-H` will start with `Get-Help`, then `Get-History`, etc.
- Enter `Get-Help` before the command you're curious about to get the manual page.^[`Get-Help Get-Help` will get help on the `Get-Help` command.]

> You can also get the equivalent of [[Tool Deep-Dives/Linux/less\|less]] by piping the output to `out-host -paging`

### PowerShell Critical Commands
- `Get-Command`
	- Get a list of all available commands
	- Can be filtered by Noun or Verb, and strings can use `*` as a wildcard
		- e.g., `*wi*ws*` will return anything with the word `Windows` in it, and any other string that matches
- `Get-Help <Command>`
	- Returns the manual page of the specified command
	- In line with this, `Update-Help` makes sure that you get the latest information when getting help on a certain command
- `Set-ExecutionPolicy`
	- Configure security policy for running scripts on the computer
	- More detail below
- Output Shaping
	- `Out-Host -Paging`
		- Equivalent to the [[Tool Deep-Dives/Linux/less\|less]] command in Linux
	- `Format-List`
		- Format the output as a list of values, grouped by object
	- `Format-Table`
		- Format the output as a table with properties as columns and objects as rows
	- `Sort-Object` (alias: `Sort`)
		- Sort objects by specific properties, delimited by commas 
			- e.g., `Get-CimInstance Win32_Process | Select-Object Name, ParentProcessId, ProcessId | Sort ParentProcessID,Name` to collect a list of all processes and sort them by their ParentProcessID, and then by Name

## Set-ExecutionPolicy
- By default, Windows computers have a **Restricted** [Execution Policy](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4)^[Non-Windows computers running PowerShell are *Unrestricted* by default.] that do not let you run unsigned PowerShell scripts.
	- This is helpful in preventing unwitting home users from hurting themselves, but it's almost useless in security
	- For example, the following one-liner from command prompt bypasses the policy:
		- `powershell.exe -ExecutionPolicy Bypass -File .\script.ps1`
- You can manually change the policy using `Set-ExecutionPolicy`
	- `Bypass` and `Unrestricted` are the most open
	- `AllSigned` or `RemoteSigned` allow signed scripts to run
		- `RemoteSigned` allows unsigned scripts if they are unblocked by something like the `Unblock-File` cmdlet.

## WMI/CMI Commands
The commands below are equivalent to the [[Tool Deep-Dives/Windows/wmic#WMIC Commands\|WMIC commands]] for process investigation
1. Get list of all processes
	1. `Get-WmiObject Win32_Process | Select-Object *`
	2. `Get-CimInstance Win32_Process | Select-Object *`
2. Get list of process names, parent process IDs, and process IDs
	1. `Get-WmiObject Win32_Process | Select-Object Name, ParentProcessId, ProcessId`
	2. `Get-CimInstance Win32_Process | Select-Object Name, ParentProcessId, ProcessId`
3. Get process and instance ID
	1. `Get-WmiObject Win32_Process -Filter "ProcessId = [PID]" | Select-Object CommandLine`
	2. `Get-CimInstance Win32_Process -Filter "ProcessId = [PID]" | Select-Object CommandLine`



# Metadata

### Sources


### Tags
#tools_win 


