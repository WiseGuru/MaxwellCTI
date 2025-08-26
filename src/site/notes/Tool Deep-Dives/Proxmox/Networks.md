---
{"dg-publish":true,"permalink":"/tool-deep-dives/proxmox/networks/","updated":"2024-02-20T10:45:59.000-08:00"}
---

Troubleshooting a bad NIC

For some reason, Proxmox doesn't want to list interface MAC addresses in the GUI, which would be a big Quality of Life change.

>  HINT: if you are looking in your network panel and you see interfaces that have *Active* set to **No**, it could Proxmox was unable to activate it.

Here are ways to gather information to troubleshoot your NIC:
1. Check status of the the Proxmox network services
	1. Shell>`systemctl status networking.service`
		1. If there are errors, it will clearly label them here, like this.
		2. ![Networks-1.png](/img/user/Attachments/Networks-1.png)
2. Take a screenshot of (or in some other way record) the *System>Network* screen for easy reference
	1. Also keep in mind which virtual interfaces/bridges your vGuests are using.
3. Get all MACs and interface names from the Shell
	1. Navigate to your node, then Shell, then `ip add` (or `ip a`)^[As mentioned in the [[Tool Deep-Dives/Linux/Linux\|Linux]] dive, `ifconfig` is being deprecated, so it's not available by default in Proxmox.] to show all available interfaces
4. Check your router's IP/Device/ARP table for connected devices.
	1. In PFsense (and I believe in OPNsense) you can go to *Diagnostics>ARP Table*