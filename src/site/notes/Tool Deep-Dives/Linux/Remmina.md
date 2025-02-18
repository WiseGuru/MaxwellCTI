---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/remmina/"}
---


#### Remmina
- A [[Tool Deep-Dives/Linux/Linux\|Linux]]-based remote desktop application that supports various protocols
- When using RDP to connect to a host over an SSH tunnel, I've found it's important to add the [[Tool Deep-Dives/SSH/SSH\|SSH]] private key with `ssh-add`, or it crashes.
- You can export configured sessions as RDP files, but if you are backing up Remmina or using it across multiple systems, you can find the complete configs in `~/.local/share/remmina/` as `*.remmina`
	- You can use a tool like [[Syncthing\|Syncthing]] or [[Tool Deep-Dives/Linux/Resilio Sync on Ubuntu 22.04\|Resilio Sync]] to automatically copy/backup the files between systems





# Metadata

### Sources
[Remote desktop client with RDP, SSH, SPICE, VNC, and X2Go protocol support. - Remmina](https://remmina.org/)
### Tags
#tools_linux 