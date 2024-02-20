---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/topics/socc-02-linux/"}
---

# Become the Master Triager
Ping/Port/Parse (14:00)
- Ping
	- Is the device alive?
- Port
	- Is the port/process alive and running?
- Parse
	- Parse those log files to figure out what's happening

# Learning Bash is critical
John Strand recommended a book, but I missed it, and can't be arsed to find it in the video. It maybe the Amazon link, but anyway, here are some sources to get started:

- [Linux Basics for Hackers | No Starch Press](https://nostarch.com/linuxbasicsforhackers)
- [Black Hat Bash | No Starch Press](https://nostarch.com/black-hat-bash)
- [UNIX and Linux System Administration Handbook: Nemeth, Evi, Snyder, Garth, Hein, Trent, Whaley, Ben, Mackin, Dan: 9780134277554: Amazon.com: Books](https://www.amazon.com/UNIX-Linux-System-Administration-Handbook/dp/0134277554)
- [Terminus](https://web.mit.edu/mprat/Public/web/Terminus/Web/main.html)
	- A terminal based game where you navigate around MIT
- [Introduction to Linux – Full Course for Beginners - YouTube](https://www.youtube.com/watch?v=sWbUDq4S6Y8)
- [Linux for Hackers | hackers-arise](https://www.hackers-arise.com/linux-fundamentals)


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/linux/#linux" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



# Linux
- It's the OS that's in basically everything, so get used to it.
- It comes in a variety of distributions or "distros"
	- Don't get distracted with the distros
		- Most of them are built on a core version (like Debian or Arch) and have various quality-of-life or customization features added (like Ubuntu, Mint, SteamOS, or Kali.)
	- There are many Unix-like OS's, like Linux, macOS, and FreeBSD
		- They often function similarly, but will have different command structures for the same task, like app installation or network interface configuration
- Users and privileges
	- Becoming SU
		- `sudo su -`
			- This changes the terminal environment from running as user AS root, to actually BEING root

### File directories
| Root Directory | Description | Notes |
| ---- | ---- | ---- |
| /bin | User binaries |  |
| /boot | Boot loader files |  |
| /dev | Device files |  |
| /etc | System configuration files |  |
| /home | User home directories |  |
| /lib | Libraries and kernel modules |  |
| /media | Removeable media mount point | Should have just been /mnt, but HERE WE ARE |
| /mnt | Mount point for temporary mounted file systems |  |
| /opt | Add-in application software packages |  |
| /sbin | System binaries |  |
| /srv | Data for services |  |
| /tmp | Temporary files |  |
| /usr | User utilities and applications |  |
| /var | Variable files |  |
| /root | Root user home directory |  |
| /proc | Virtual file system |  |

### Navigating Linux and directories
- `cd`
	- Change directory
	- Running without any changes will send you back to the home directory
- `ls`
	- Show any visible (non-hidden) files
	- `ls -lrta`
		- Shows hidden files
- `mkdir`
	- create directory
- `locate [string]`
	- It will search a file index to look for and list any locations
	- `sudo update db`
		- Update the index
- `ps aux`
	- List processes


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/vi/#vi" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### vi
- Universally available text editor in [[Tool Deep-Dives/Linux\|Linux]]
	- Not [[VIM\|VIM]], which stands for "vi iMproved"
- `vi [file name]`
	- Opens a file in "vi" text editor
	- vi editing commands
	    - `a` - start editing
	    - `Esc` - stop editing
	    - `:` - Command for vi
	    - `:wq!` - quit
	        - `w` - write
	        - `q` - quit
	        - `!` - force/ignore errors







</div></div>


### Running Processes
1. `ps aux`
	1. Shows all processes
		1. a - all processes
		2. u - sorted by user
		3. x - include the processes using a teletype terminal
	2. [How to Use the ps aux Command in Linux | Linode Docs](https://www.linode.com/docs/guides/use-the-ps-aux-command-in-linux/)
2. `top`
	1. Shows *live* processes

### Networking
1. `ip a`
	1. New version of `ifconfig`, shows network interface configuration and various stats
		1. If it's not installed, you can install the `iproute2` package from apt
	2. `ifconfig` is no longer installed by default on newer versions of Linux, so you should get used to running `ip a`
		1. However, if necessary, you can install `net-tools` (`sudo apt install net-tools`) to get it back

[[Tool Deep-Dives/Nmap\|Nmap]]


</div></div>





## Linux
3. Networking stuff
	3. NMAP and checking remote ports
		1. Run as `su`
		2. Port/State/Service
			1. Port
				1. The port is the thing
			2. State
				1. Open
					1. The port is verifiably open
				2. Closed
					1. The port is verifiably closed
				3. Open|filtered
					1. The port may be open
			3. Service
		3. Q from audience: How often do you scan for UDP?
			1. A: NMAP sends specific information to UDP ports (vs. blank packets) to initiate a conversation, based on the appropriate protocol
				1. e.g., port 53 will be a DNS request, 161 will be an SNMP request, etc.
	4. `netstat`
	5. List open files `lsof`
		1. `lsof -i -P`
			1. Shows all open files and their connected shit
			2. pipe, backpipe, what port it's listening to, etc.
4. **Show and evade Linux history**
	1. `history`
		1. Shows complete bash command history
	2. add a space before the command
		1. `~$ ps aux`
			2. Will appear in the history
		2. `~$  ps aux`
			1. Will NOT appear in the history
5. Files and file management
	1. `cd /proc`
		1. `/proc` is a virtual file system that contains information about processes and etc.
6. `strings`

### Lab: [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/BHIS-SOCC-lab-LinuxCLI\|Linux]]
