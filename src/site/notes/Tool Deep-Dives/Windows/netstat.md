---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/netstat/"}
---

#### netstat
- *netstat* (or network statistics) is a cross-platform CLI tool to monitor network traffic
	- Unfortunately, some of the switches are different depending on the OS, and and the specifics can be found on the [Wikipedia Page](https://en.wikipedia.org/wiki/Netstat#Parameters)
	- It is mostly obsolete on [[Tool Deep-Dives/Linux/Linux\|Linux]], and was replaced with [[Tool Deep-Dives/Linux/ss\|ss (secure socket)]]. 
- It shows the live connections to the host system

## netstat Windows Commands
1. `netstat`
	1. `-naob`
		2. `a` - displays all active TCP and connections and TCP/UDP ports
		3. `n` - displays active TCP connectives, expressed numerically, no attempt to determine name
		4. `o` - displays active TCP connections with their PID
		5. `b` - displays the executable involved
			1. **NOTE**: The executable is listed *after* all the connection it's made
			2. ![Day 2-4.png](/img/user/Attachments/Day%202-4.png)
			3. **NOTE**: On macOS, `b` reports the total bytes in the traffic
	2. `-f`
		1. *Windows specific*; shows FQDN, may need to be run multiple times
		2. Can be helpful in identifying services that don't resolve to a FQDN, and are suspicious
		3. However, also takes longer to run, so not necessarily helpful for broad traffic filtering




# Metadata

### Sources
[netstat - Wikipedia](https://en.wikipedia.org/wiki/Netstat)

### Tags
#tools_win
