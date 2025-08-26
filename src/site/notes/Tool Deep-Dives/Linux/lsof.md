---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/lsof/","updated":"2024-08-26T12:39:10.408-07:00"}
---

#### lsof
- `lsof` stands for *list open files*
	- Typically best run as `sudo` to capture all files
- Incredibly helpful in understanding what all is connected and how they're connected.

1. Usage: `lsof`
	1. `-i` is internet connections
	2. `-P` port number only
		1. e.g., `22` and not `ssh`
	3. `-p [process id]` shows everything tied to a specific process ID (PID) 

![BHIS-SOCC-lab-LinuxCLI-1.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-1.png)

![BHIS-SOCC-lab-LinuxCLI-2.png](/img/user/Attachments/BHIS-SOCC-lab-LinuxCLI-2.png)



# Metadata

### Sources
[lsof(8) - Linux manual page](https://man7.org/linux/man-pages/man8/lsof.8.html)
[How to use the lsof command to troubleshoot Linux | Enable Sysadmin](https://www.redhat.com/sysadmin/analyze-processes-lsof)

### Tags
#tools_linux 