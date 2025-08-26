---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/topics/socc-02-linux/","updated":"2024-02-20T11:02:22.000-08:00"}
---

# Become the Master Triager
Ping/Port/Parse (14:00 of the stream)
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



<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/linux/linux/#linux" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



# Linux
*On Linux, everything is a file.*
- It's the OS that's in basically everything, so get used to it.
	- There's also the Linux [kernel](https://en.wikipedia.org/wiki/Kernel_(operating_system))
- It comes in a variety of distributions or "distros"
	- Don't get distracted with the distros
		- Most of them are built on a core version (like Debian or Arch) and have various quality-of-life or customization features added (like Ubuntu, Mint, SteamOS, or Kali.)
	- There are many Unix-like OS's, like Linux, macOS, and FreeBSD
		- They often function similarly, but will have different command structures for the same task, like app installation or network interface configuration
- Users and privileges
	- Becoming SU
		- `sudo su -`
			- This changes the terminal environment from running as user AS root, to actually BEING root

### Resources if you get lost
> **WHEN IN DOUBT**: You can always learn about a command with by running `man <command>`, like `man pwd`

- [die.net](https://www.die.net/)
	- Various Linux manual pages
- [explainshell.com](https://explainshell.com/)
	- Copy and paste an entire line of shell commands to have each section broken down and identified clearly
### File directories
| Root Directory | Description                                    | Notes                                       |
| -------------- | ---------------------------------------------- | ------------------------------------------- |
| /boot          | Boot loader files                              |                                             |
| /dev           | Device files                                   |                                             |
| /etc           | System configuration files                     |                                             |
| /home          | User home directories                          |                                             |
| /lib           | Libraries and kernel modules                   |                                             |
| /media         | Removeable media mount point                   | Should have just been /mnt, but HERE WE ARE |
| /mnt           | Mount point for temporary mounted file systems |                                             |
| /opt           | Add-in application software packages           |                                             |
| /bin           | User binaries                                  |                                             |
| /sbin          | System binaries                                |                                             |
| /srv           | Data for services                              |                                             |
| /tmp           | Temporary files                                |                                             |
| /usr           | User utilities and applications                |                                             |
| /var           | Variable files                                 |                                             |
| /root          | Root user home directory                       |                                             |
| /proc          | Virtual file system                            |                                             |

### Navigating Linux and directories
- `cd`
	- Change directory
	- Running without any changes will send you back to the home directory
	- `cd /proc`
		- `/proc` is the *virtual file system*, and contains information about running processes
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



<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/linux/vi/#vi" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### vi
- Universally available text editor in [[Tool Deep-Dives/Linux/Linux\|Linux]]
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

### Networking and file management
1. `ip a`
	1. New version of `ifconfig`, shows network interface configuration and various stats
		1. If it's not installed, you can install the `iproute2` package from apt
	2. `ifconfig` is no longer installed by default on newer versions of Linux, so you should get used to running `ip a`
		1. However, if necessary, you can install `net-tools` (`sudo apt install net-tools`) to get it back
2. `netstat`
	1. Displays active network connections
		1. `a` - all listening and non-listening (e.g. established) connections
			1. Listening sockets are waiting for a connection
			2. Established sockets have an active connection
		2. `t` - TCP connections
		3. `u` - UDP connections
3. `lsof`
	1. List open files
	2. `lsof -i -P`
		1. Shows all open files and their connected devices
		2. `i` - Shows all open network files
			1. You can also specify an IP address to narrow it down to a specific host
		3. `P` - Prohibits convert port numbers to names
			1. Can make `lsof` run faster, 
	3. pipe, backpipe, what port it's listening to, etc.

[[Tool Deep-Dives/Nmap\|Nmap]]

### Linux Terminal History
1. `history`
	1. Shows a complete history of Bash commands
	2. Relatively easy to evade, add a space before the command
		1. `~$ ps aux`
			2. Will appear in the history
		2. `~$  ps aux`
			1. Will NOT appear in the history



</div></div>



### Lab: [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-LinuxCLI\|Linux]]
