---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/labs/bhis-socc-lab-memory-analyses/","updated":"2025-06-09T11:43:13.699-07:00"}
---


> **NOTE**: This lab has changed names since I last 
## LAB: Memory Forensics
[MemoryAnalysis(Volatility)](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/Memory/MemoryAnalysis(Volatility).md)

In this lab, we're looking at a memory dump of a compromised Windows system. To gather similar information in the wild, you can [[Tool Deep-Dives/Windows/Windows#Getting Better Memory Dumps\|get better memory dumps by following this guide]].

### Lab work
1. Extract the memory dump
	1. The first half of the lab is getting the memory dump extracted and ready to investigate.
2. Running [[Tool Deep-Dives/Volatility\|Volatility]]
	1. `python3 vol.py -f /mnt/c/tools/volatility_2.6_win64_standalone/memdump.vmem windows.netscan`
		1. `python(3) vol.py`
			1. Runs the Volatility tool; in this lab, we're using Volatility 3, Framework 1.0.0
		2. `-f <file name>`
			1. Identifies the file being investigated, in this case, `memdump.vmem`
		3. `windows.x`
			1. The Windows module or plugin being used to analyze the files; in this particular case, the [netscan module](https://volatility3.readthedocs.io/en/v2.0.1/volatility3.plugins.windows.netscan.html).
			2. Each module/plugin can significantly change the output, so I'm going to break out each module into its own deal and explain what it's doing
		4. It can take a few minutes for Volatility to produce an output.
			1. If you are running this at home (and you have the cores to spare), you can open multiple Ubuntu terminal tabs and navigate them all to the same place (after doing the initial extraction): `cd /mnt/c/IntroLabs/volatility3-1.0.0/`
	2. `windows.netscan`
		1. The Windows Netscan module provides information of network connections at the time of the dump
			1. The key ones to look for are `CLOSED` and `ESTABLISHED`
				1. These are connections that *were* open at the time of the dump, or were open *recently*.
			2. The output is long, but we can tighten it up.
		2. Trimming the search
			1. **WARNING**: An attacker could theoretically perform their activity in such a way that *excluding* key phrases with tools like [[Tool Deep-Dives/Linux/grep\|grep]] might cause them to reject
				1. For example, the malicious application `LISTENING.exe` would get culled from the output by `| grep -v LISTENING`
				2. Therefore, it would be worth running multiple commands at once to cover all blindspots and efficiently go through the data
			2. The raw output is also pretty visually mangled; we can pipe to the [[Tool Deep-Dives/Linux/column\|column]] and [[Tool Deep-Dives/Linux/less\|less]] Linux commands to straighten the output and and it a little easier to read.
		3. Possible search parameters
			1. `| grep -v '0.0.0.0\|::'`
				1. Removes all the listening sockets
				2. In this example, condenses the output from 2 pages to 1/3rd of a page
			2. `| grep 'CLOSED\|ESTABLISHED'`
				1. Just identifies any closed or open connections
			3. `| grep -w '21\|22\|23\|25\|80\|135\|139\|443\|445\|3389'`
				1. Search for the OWASP Top 10 ports listed in [[Webinars and Training/BlackHills SOC Core/Topics/SOCC01 - Networking and PCAPs\|SOCC01 - Networking and PCAPs]].
				2. When I first tried this command, it returned search results from all over the output, little of which was helpful
					1. ![BHIS-SOCC-lab-MemoryForensics.png](/img/user/Attachments/BHIS-SOCC-lab-MemoryForensics.png)
				3. Adding the `-w` switch searched instead of the complete "word" instead of just the string, and it cut out a ton of useless information.
					1. ![BHIS-SOCC-lab-MemoryForensics-1.png](/img/user/Attachments/BHIS-SOCC-lab-MemoryForensics-1.png)
		4. Findings
			1. After running our searches, using the CLOSED/ESTABLISHED grep, we get the following output
				1. ![BHIS-SOCC-lab-MemoryForensics-2.png](/img/user/Attachments/BHIS-SOCC-lab-MemoryForensics-2.png)
			2. Things we are concerned about:
				1. An established `445` SMB connection to another computer on the network, begun by the System process^[While investigating the [[Tool Deep-Dives/Volatility\|Volatility]] tool, I found out that the "System" and "smss" processes will not have session IDs, because System starts before sessions are established, and SMSS is the session manager. [GitHub, Volatility: pslist](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#pslist)]
					1. The Process ID (PID) of 4
					2. If we search through the output for more connections that computer, we don't find anything.
					3. Given that it's System, we're unlikely to find much directly linking it to itself.
				2. A few connections to an external IP over an unknown port 4444 from an application we don't recognize^[I know it's fun, but I do wish in this lab the exe was a little more normalized; like what if it was called "svchost.exe" or "WinSearchUI.exe" or something like that? Feels more realistic, and you're less likely to automatically count it as suspicious because it has a goofy name.]
					1. PID is 5452
	4. `windows.pslist`
		1. We'll use this module to check on all running processes
			1. You can scroll through to find suspicious ones, but it might be better at this stage to just search for the PIDs we found earlier
		2. More trimming! Search for the suspicious PIDs
			1. `python3 vol.py -f /mnt/c/tools/volatility_2.6_win64_standalone/memdump.vmem windows.pslist | grep -w '4\|5452'`
			2. As we search, we could add or remove PIDs from the grep
		3. Findings
			1. From this output, we can see that the *TrustMe.exe* process was started in *cmd.exe*, and this is suspicious
				1. Users do not run CMD on a day-to-day basis, so that's another red flag
			2. As expected, not a ton on the System side, though we may find more later on.
	5. `windows.pstree`
		1. This gives us the process tree, showing all processes and their relationships to each other
		2. Diving into one of the CMD processes, we find it tied back to `TrustMe.exe`, along with a few tagalongs
		3. ![BHIS-SOCC-lab-MemoryForensics-3.png](/img/user/Attachments/BHIS-SOCC-lab-MemoryForensics-3.png)
			1. PID on the left, Parent PID (PPID) on the right
			2. This information is available in `pslist`, but it's not as visually apparent.
	6. `dllist --pid 5452`
		1. Shows which DLLs are associated with PID 5452
			1. We can see what files were loaded, when, and where from
		2. Findings
			1. TrustMe.exe was run from the Downloads folder, so that's also suspicious
	7. `windows.malfind.Malfind`
		1. The "easy button" to identify malware; it checks for a specific kind of hidden/injected DLL using [[Definitions and Topics/VAD\|VAD]] tags
			1. Sadly, `column -t` just really screws up the output...
		2. Findings
			1. Turns out, TrustMe.exe is bad!
			2. There are 4 instances with highly suspicious access to memory regions