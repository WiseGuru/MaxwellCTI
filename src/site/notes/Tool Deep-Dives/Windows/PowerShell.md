---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/power-shell/"}
---


#### PowerShell
- *PowerShell* is the defacto [[Tool Deep-Dives/Windows/Windows\|Windows]] CLI management tool
	- While it's a little more cumbersome than other programming tools, it's much easier to learn and read by non-specialists.^[Though some common commands have aliases that match the shorter version.]
		- For example, `cd .\Tools` is impenetrable for someone who doesn't know how `cd` works, but `Set-Location -Path C:\Tools` literally spells it out for you.
- Commands are constructed with a `Verb-Thing` syntax
	- e.g., `Get-ADUser` to retrieve information for a user in [[Tool Deep-Dives/Windows/Active Directory\|Active Directory]] or `Set-ADUser` to modify that user
- If you're not sure of the full command,^[Or you don't want to type the whole thing out.] you can use "Tab" to autocomplete based on the available information.
	- Continuing to press tab will cycle through the available commands.
	- Example, `Get-H` will start with `Get-Help`, then `Get-History`, etc.
- Enter `Get-Help` before the command you're curious about to get the manual page.^[`Get-Help Get-Help` will get help on the `Get-Help` command.]

> You can also get the equivalent of [[Tool Deep-Dives/Linux/less\|less]] by piping the output to `out-host -paging`

## Set-ExecutionPolicy
- By default, Windows computers have a **Restricted** [Execution Policy](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4)^[Non-Windows computers running PowerShell are *Unrestricted* by default.] that do not let you run unsigned PowerShell scripts.
	- This is helpful in preventing unwitting home users from hurting themselves, but it's almost useless in security
	- For example, the following one-liner from command prompt bypasses the policy:
		- `powershell.exe -ExecutionPolicy Bypass -File .\script.ps1`
- You can manually change the policy using `Set-ExecutionPolicy`
	- `Bypass` and `Unrestricted` are the most open
	- `AllSigned` or `RemoteSigned` allow signed scripts to run
		- `RemoteSigned` allows unsigned scripts if they are unblocked by something like the `Unblock-File` cmdlet.


# Metadata

### Sources


### Tags
#tools_win 


