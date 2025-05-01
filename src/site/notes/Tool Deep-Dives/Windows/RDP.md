---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/rdp/","noteIcon":""}
---

#### RDP
- *Remote Desktop Protocol* (*RDP*) was developed by Microsoft for users establish remote connections
- Uses TCP/UDP port 3389
	- Kids (and adults) who don't know any better will open port 3389 on their public router, allowing attackers to brute-force and remotely access their desktop.
- There are some valid security concerns to using RDP, so it's generally recommended to only allow RDP connections from the local network (allowing remote access via a VPN), and in public/untrusted networks, only after first connecting to the machine with a more secure protocol like [[Tool Deep-Dives/SSH/SSH\|SSH]].
	- Bear in mind that without an SSH, an attacker with access to a trusted network would be able to exploit RDP vulnerabilities more easily.




# Metadata

### Sources
[Remote Desktop Protocol - Wikipedia](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol)
### Tags
#tools_win 