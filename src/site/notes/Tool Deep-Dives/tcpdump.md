---
{"dg-publish":true,"permalink":"/tool-deep-dives/tcpdump/","tags":["tools_sec"]}
---

#### tcpdump
- Unix/Linux CLI-based packet capture and analyzer utility that is extremely flexible
	- Also fun fact, the `man` page for `tcpdump` breaks down the [TCP header](https://ccnadefinitions.com/ccna/20-definitions/tcp/) 
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
- Like Wireshark, you can also filter output by net, host, and/or port
	- `tcpdump host 192.168.1.2 port 80`
	- `tcpdump net 192.168.1.0/24 port 445`
- **CRITICALLY**, the output can be managed more easily when piped to various Linux commands
	- [[Tool Deep-Dives/Linux/less\|less]]
	- [[Tool Deep-Dives/Linux/grep\|grep]]
		- Specifically, you can use `\|` to grab multiple variables, like `grep '3389\|445\|CLOSED\|ESTABLISH'
- **Flags**
	- [reading tcp flags](https://gist.github.com/tuxfight3r/9ac030cb0d707bb446c7)

### Work
[[BHIS Antisyphon and Webinars/BlackHills SOC Core/Topics/SOCC01 - Networking and PCAPs\|SOCC01 - Networking and PCAPs]]
[[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/BHIS-SOCC-lab-tcpdump\|BHIS-SOCC-lab-tcpdump]]


# Metadata

### Sources
[tcpdump(1) man page | TCPDUMP & LIBPCAP](https://www.tcpdump.org/manpages/tcpdump.1.html)
[tcpdump - Wikipedia](https://en.wikipedia.org/wiki/Tcpdump)
[A tcpdump Tutorial with Examples](https://danielmiessler.com/p/tcpdump/)