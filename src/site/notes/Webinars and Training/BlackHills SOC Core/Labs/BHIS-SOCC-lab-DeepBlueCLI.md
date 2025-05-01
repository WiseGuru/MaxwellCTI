---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/labs/bhis-socc-lab-deep-blue-cli/","noteIcon":""}
---


## LAB: DeepBlueCLI
[IntroLabs/DeepBlueCLI](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/deepbluecli/DeepBlueCLI.md)

In this lab, we're using [[Tool Deep-Dives/DeepBlueCLI\|DeepBlueCLI]] to investigate Event Logs and search for patterns.

### DeepBlue CLI lab
It looks like this is mostly just pre-made .evtx files^[evtx files are exported Windows Event Log files in XML format.] with examples of information one would find.

This feels like event log analysis on easy-mode; you basically just run DeepBlue against the event log file, it thinks really hard, and then outputs what it finds suspicious. However, some of the log files don't feel particularly helpful in demonstrating the power of the tool.

1. Access the tool
	1. Run PowerShell^[For some reason the official lab has you run Command Prompt as an admin, and then have it open PowerShell from the prompt, and I don't know why not run PowerShell as admin first...] as an admin and navigate to the DeepBlueCLI folder.
		1. `cd \tools\DeepBlueCLI-master`
	2. Set the Execution Policy to "Unrestricted"
		1. `set-executionpolicy Unrestricted`^[For more detail, check out [[Tool Deep-Dives/Windows/PowerShell#Set-ExecutionPolicy\|Set-ExecutionPolicy]]]
2. Inspect the Event Log Files for suspicious events
	1. `.\DeepBlue.ps1 .\evtx\new-user-security.evtx`
		1. What do we find?
			1. The user named named `-` (just a dash) added to the Administrator group
			2. New user `IEUser` is created
			3. Both user accounts have the same [[Definitions and Topics/SID\|SID]]
			4. The output from DeepBlue is much cleaner than the information what we see in Event Viewer, which is really neat.
		2. I think this is a weird example to demonstrate the tool
			1. There are only 4 events total in the EVTX file total^[We can open Event Viewer, and under "Actions" at the top right, select "Open Saved Log..." to view the logs in their natural environment.], and DeepBlue identified two of them as suspicious.
			2. I suppose that alone is pretty cool, but I feel like having more data for it to to parse through would be more interesting.
	2. `.\DeepBlue.ps1 .\evtx\password-spray.evtx`
		1. Right away, this is much cooler test.
			1. If we open the file in Event Viewer, we see there are 295 events that occurred over 5 minutes
				1. Perusing the logs, we see that almost all of the logs show user `jwrig` attempting to log in to a bunch of different users and failing over and over again
			2. DeepBlue outputs a single message summarizing what's going on.
				1. It identifies the kind of attack (Password Spray Attack)
				2. It defines the attack
				3. It says which accounts were targeted
				4. It identifies the user and computer originating the login attempts
	3. `.\DeepBlue.ps1 .\evtx\smb-password-guessing-security.evtx`
		1. Again, super interesting; it analyzed all the logs and spat out two reports for a high number of logon failures for multiple accounts
			1. We're told how many logon failures there are and which account is related (maybe)
		2. For the first report, it specifically calls out the Administrator as the account with failed login attempts, but the second report only gives the total number of accounts with failed logins.
		3. Looking at the files in Event Viewer, I noticed that everything except for the first log entry was for the administrator account
	4. `.\DeepBlue.ps1 .\evtx\Powershell-Invoke-Obfuscation-encoding-menu.evtx`
		1. This is where it gets really cool
		2. This event log contains a ton of obfuscated PowerShell, and the output is the most suspicious ones based on length and ratio of alphanumeric and common symbols to other symbols.
			1. ![BHIS-SOCC-lab-DeepBlueCLI.png](/img/user/Attachments/BHIS-SOCC-lab-DeepBlueCLI.png)
				1. Sample output
		3. We also see a range of obfuscated and [[Tool Deep-Dives/Base64\|Base64]]-encoded [[Tool Deep-Dives/Windows/PowerShell\|PowerShell]] commands. Yikes! 

Anyway, really interesting lab, and really well crafted tool. There are a many more EVTX files to run [[Tool Deep-Dives/DeepBlueCLI\|DeepBlueCLI]] against and see what different kinds of activity looks like.

## Additional Sources
[Quick Forensics of Windows Event Logs (DeepBlueCLI) - YouTube](https://www.youtube.com/watch?v=G8XjSO_eshc)