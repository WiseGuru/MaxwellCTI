---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/ss/","noteIcon":""}
---

#### ss
- *ss (secure socket)* is used to investigate socket statistics, and can show similar information to [[Tool Deep-Dives/Windows/netstat\|netstat]]

### Commands
- `ss`
	- `ss -tuln`
		- `t` - Display TCP sockets
		- `u` - Display UDP sockets
		- `l` - listening sockets
		- `n` - Numeric (don't resolve service names)
	- `ss -o state established`
		- `o` - SSH connections
		- `state` - Filters results by the state of the socket (e.g. established, listening, closed, etc.)



# Metadata

### Sources
[ss(8) - Linux manual page](https://man7.org/linux/man-pages/man8/ss.8.html)

### Tags
#tools_linux 