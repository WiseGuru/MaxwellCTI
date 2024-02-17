---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/bhis-socc-notes/"}
---

First, this course was great; tons of great information, the brains of the brightest ripe for picking, and an infectiously energetic atmosphere.

I was able to attend all 4 days live and perform all the labs etc., and because I don't want to take away from their courses, I'm going to experiment with covering my impressions and give a kind of course overview.

# Day 1

## Section  1 - IP/TCP Headers
- General discussion of the IP header
	- Routers predominately are the devices that only look at the IP Header
- IP Header
	- [BGP Hijacking](https://www.cloudflare.com/learning/security/glossary/bgp-hijacking/)
		- APT/Nation-state actor can compromise an [ASN](https://www.cloudflare.com/learning/network-layer/what-is-an-autonomous-system/) and redirect traffic to another destination
		- *Traffic is routed to the most specific address*
	- Fragmentation flags
		- x - bit 0
			- ["the evil bit"](https://en.wikipedia.org/wiki/Evil_bit)
			- Reserved, but be 0
		- D (DF) - bit 1
			- Don't fragment
		- M (MF) - bit 2
			- More Fragments
- TCP Header
	- Describes the ports for the packets
		- Ports are *generally* used by certain protocols, but *generally*, ports can be used by any service
		- Port **0** can be used, and has been used as a 
	- [NAT/PAT](https://ccnadefinitions.com/ccna/20-definitions/nat/)
	- RFC 793 describes the 3-way handshake
		- Originally, it was a 4-way handshake, but most modern OS's send a 3-way instead.
### Top 10 ports
| Port | Protocol | Interesting Links |
| ---- | ---- | ---- |
| 80 | HTTP |  |
| 23 | Telnet |  |
| 22 | SSH |  |
| 443 | HTTPS |  |
| 3389 | ms-term-serv (RDP) |  |
| 445 | microsoft-ds (SMB) | SMB is synonymous in my head with EternalBlue [EternalBlue - Wikipedia](https://en.wikipedia.org/wiki/EternalBlue) |
| 139 | netbios-ssn | [137,138,139 - Pentesting NetBios - HackTricks](https://book.hacktricks.xyz/network-services-pentesting/137-138-139-pentesting-netbios) |
| 21 | FTP |  |
| 135 | MSRPC | [Microsoft RPC - Wikipedia](https://en.wikipedia.org/wiki/Microsoft_RPC) |
| 25 | SMTP |  |
### Shodan top ports
| Port | Protocol | Notes |
| ---- | ---- | ---- |
| 80, 8080, 443, 8443 | HTTP/S |  |
| 21 | FTP |  |
| 22 | SSH |  |
| 23 | Telnet |  |
| 161 | SNMP |  |
| 143, 993 | IMAP/Encrypted |  |
| 25 | SMTP |  |
| 5060 | SIP |  |
| 554 | RTSP (Real Time Streaming Protocol) |  |


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/tcpdump/#tcpdump" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### tcpdump
- Unix/Linux CLI-based packet capture and analyzer utility that is extremely flexible
- Not as feature-right as [[Tool Deep-Dives/Wireshark\|Wireshark]], but can run in more places with less overhead more quickly
- Important commands
	- `-D`
		- Lists interfaces
	- `-XA`
		- X is for Hex
		- A is for ASCII
	- `-w`
		- Write the data to a file
	- `-n`
		- Don't resolve hostname
	- `-r <file>`
		- Identify the file you're reading from
- Like Wireshark, you can also filter output by host and port
	- `tcpdump host 192.168.1.2 port 80`
- **CRITICALLY**, the output can be managed more easily when piped to various Linux commands
	- [[Tool Deep-Dives/less\|less]]
	- [[Tool Deep-Dives/grep\|grep]]
		- Specifically, you can use `\|` to grab multiple variables, like `grep '3389\|445\|CLOSED\|ESTABLISH'









</div></div>



<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



- More PCAPs to analyze:
	- [TCPdump Labs](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/TCPDump/TCPDump.md)

</div></div>




<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/#wireshark-hot-tips" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Wireshark Hot Tips
- KEY: In a conversation, right click a packet and select **Follow>TCP Stream** to show the *data that is being sent between hosts*  🤯
- KEY: you can right-click, select **HTTP>Requests** to see *the malicious crap that's flying around*





</div></div>




<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/linux/#linux" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



# Linux
- It's the OS that's in basically everything, so get used to it.
- It comes in a variety of distributions or "distros"
	- **insert distinction between Linux and Unix here**
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









</div></div>

