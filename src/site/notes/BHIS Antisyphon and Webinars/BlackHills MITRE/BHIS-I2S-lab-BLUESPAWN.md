---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-mitre/bhis-i2-s-lab-bluespawn/"}
---


## LAB: BLUESPAWN
[IntroLabs/IntroClassFiles/Tools/IntroClass/bluespawn/Bluespawn.md at master · strandjs/IntroLabs · GitHub](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/bluespawn/Bluespawn.md)

In this lab, we're looking at adversary emulation with [[Tool Deep-Dives/BLUESPAWN\|BLUESPAWN]] and [[Tool Deep-Dives/Atomic Red Team\|Atomic Red Team]].
- [[Tool Deep-Dives/BLUESPAWN\|BLUESPAWN]] functions as the EDR

### Lab work
1. Ensure Microsoft Defender isn't running with `Set-MpPreference -DisableRealtimeMonitoring $true`
	1. Should already be disabled if you ran the script at [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/00-BHIS-SOCC-lab-Config\|00-BHIS-SOCC-lab-Config]]
2. Configure/start BLUESPAWN
	1. Open CMD and start BLUESPAWN^[I'm tired of typing this in all caps already...]
		1. `cd \tools`
		2. `BLUESPAWN-client-x64.exe --monitor --level Cursory`
3. Start Atomic Red Team in PowerShell
	1. Navigate to the folder
		1. `cd C:\AtomicRedTeam\invoke-atomicredteam\`
	2. Install YAML powershell module
		1. `Install-Module -Name powershell-yaml`
		2. use `A` to agree to install everything
	3. Import the ART module
		1. `Import-Module .\Invoke-AtomicRedTeam.psm1`
4. Run the test
	1. Let it run for about 2 minutes; when done, kill it with Ctrl+C
		1. `Invoke-AtomicTest All`
		2. As it runs through, you'll see a bunch of [[Attack Frameworks/MITRE ATT&CK/MITRE ATT&CK\|MITRE ATT&CK]] reference numbers and the attack being performed
	2. While the test is running, switch to the BLUESPAWN lab to watch the results
		1. You'll see a bunch of findings and their associated [[Attack Frameworks/MITRE ATT&CK/MITRE ATT&CK\|MITRE ATT&CK]] reference numbers
	3. Cleanup
		1. `Invoke-AtomicTest All -Cleanup`

### Next Steps
Run the Meterpreter attack from [[BHIS Antisyphon and Webinars/BlackHills MITRE/Labs/BHIS-I2S-lab-Applocker\|BHIS-I2S-lab-Applocker]] and [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/BHIS-SOCC-lab-Sysmon\|BHIS-SOCC-lab-Sysmon]], and see how [[Tool Deep-Dives/BLUESPAWN\|BLUESPAWN]] reacts to it
