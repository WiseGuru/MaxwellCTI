---
{"dg-publish":true,"permalink":"/tool-deep-dives/tcpdump/","tags":["tools_soc"]}
---

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








# Metadata

### Sources
[tcpdump(1) man page | TCPDUMP & LIBPCAP](https://www.tcpdump.org/manpages/tcpdump.1.html)
[tcpdump - Wikipedia](https://en.wikipedia.org/wiki/Tcpdump)