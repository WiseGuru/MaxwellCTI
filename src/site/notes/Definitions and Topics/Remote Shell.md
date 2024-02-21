---
{"dg-publish":true,"permalink":"/definitions-and-topics/remote-shell/","tags":["defs_soc"]}
---

#### Remote Shell
- Remote Shell is the ability to run shell commands on a remote machine.
- Three are a few common methods to establish *remote shell*, and I'll try to describe them here
	- *Traditional Remote Shell*
		- The client/user computer connects to the server/remote computer via an established connection protocol, like [SSH](https://ccnadefinitions.com/ccna/20-definitions/ssh/) or *(shudder)* [Telnet](https://ccnadefinitions.com/ccna/20-definitions/telnet/)
		- The remote computer must be listening on the appropriate port, and the client must make a connection request
		- This is commonly used for benign purposes (like an administrator managing a router or headless server)
	- *Bind Shell*
		- Like a traditional remote shell, the client/user computer connects to the server/remote computer. **However**, in this case, the port is *not necessarily* then correct port
		- The attacker (it's almost always an attacker) looks for an open port on the target machine and attempts to establish a shell connection to it
		- Modern firewalls should be capable at stopping this kind of attack
	- *Reverse Shell*
		- The attacker opens a port on their (client) computer, and the server/remote computer listens for commands from that open port
		- A server can be configured to do this through [[SQL Injection\|SQL Injection]], or through a malware payload that runs [a single line of code](https://saucer-man.com/reverse/).
		- [[Botnets\|Botnets]] are often built like this, where hundreds or thousands of remote devices will listen to one or two [[Definitions and Topics/C2\|C2]] servers for instructions




# Metadata

### Sources
[Reverse shell cheatsheet](https://saucer-man.com/reverse/)