---
{"dg-publish":true,"permalink":"/tool-deep-dives/proxmox/adding-drives-to-proxmox/","updated":"2024-03-31T14:47:24.000-07:00"}
---


I'm building out a homelab, and I'd *physically* added two 10TB drives to my Proxmox server, but I wasn't sure how to *logically* add them as a mirrored drive to Proxmox.

1. From the web-gui, choose *Server View* from the top-left, then expand *Datacenter*, and select the host name
2. Select "*Disks*"
	1. If the disk has already been formatted with partitions, select *Wipe Disk*
	2. Next, select *Initialize Disk with GPT*, and get an error:
	3. ![Adding drives to Proxmox-1.png](/img/user/Attachments/Adding%20drives%20to%20Proxmox-1.png)
	4. Possible ways to fix - in Shell - *DON'T NAVIGATE AWAY*^[Navigating away closes the shell, and when you come back, it's a new terminal.]
		1. `wipefs -af /dev/sd#`
			1. The issue continues even though the command succeeded
		2. Use Parted - **Worked**!
			1. `apt update && apt dist-upgrade`^[Don't use `apt upgrade`, it can and will cause serious issues]
			2. `apt install parted`
			3. `parted /dev/sd#`
				1. `mklabel gpt`
				2. `unit TB`
				3. `mkpart primary 0.00TB 10.00TB`
				4. `print`
					1. This just shows the results
	5. Or you can just skip straight to shell and create the zpool in Shell
		1. `zpool create (Pool name) (mirror/Raid10/etc.) /dev/sd1 /dev/sd2 /dev/sdetc.`
	6. Or you can go to ZFS and just add it straight through there...