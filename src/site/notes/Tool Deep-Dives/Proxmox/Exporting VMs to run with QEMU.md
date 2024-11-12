---
{"dg-publish":true,"permalink":"/tool-deep-dives/proxmox/exporting-v-ms-to-run-with-qemu/"}
---

> **Disclaimer**: This is a long-ass process, and should really only be done if there are no other options. 

#### Summary of Operations
- Backup VM in question
	- Clone the VM is the quicker way to create a full backup (not a snapshot)
		- But creating a backup through the GUI is slower, because Proxmox tries to compress and repackage it.
- ~~Export disk via Proxmox shell to local storage~~ *Create backup of VM*
	- If your install disk is small, you will likely need to [[Tool Deep-Dives/Proxmox/Mounting USB Drives in ProxMox\|add USB storage]] and configure it to accept backups.
	- Shutdown the VM, then either through the GUI or with `vzdump`, create a "stop" backup to the drive
- Extract the `.vma` backup on ProxMox
	- *.vma is proprietary to Proxmox*, and must be extracted with their OS
		- Actually, you can run the python tool [VMA extractor](https://github.com/jancc/vma-extractor) to extract the VM Archive format.
	- `vma extract /path/to/backup.vma /path/to/destination/folder`
		- This command will create a new folder; if the folder already exists, it will complain.
- Use SCP/SFTP/USB/etc. to copy the file to your local computer
	- *Pulling* from ProxMox: `scp -r ProxMoxUser@ProxMoxIP:/path/to/source /path/to/destination`
		- **Reminder**: If you cancel a transfer, it doesn't delete the files you were copying.
		- [[Tool Deep-Dives/SSH/OpenSSH\|sshd]] is likely already configured on ProxMox, so this will probably be easier
	- *Sending* from ProxMox: `scp -r /path/to/source TargetUser@TargetIP:/path/to/destination`
		- Will require you to configure your machine as an SSH host.
- Install QEMU if necessary and run the VM.
	- [[Webinars and Training/BlackHills SOC Core/Labs/01-BHIS-SOCC-lab-LinuxHostConfig#Running Without SPICE\|Running the VM without spice]]
	- You *should* be able to run the VM without converting it; just set `format=raw` and it should work.
	

# Metadata

### Sources
[VMA - Proxmox VE](https://pve.proxmox.com/wiki/VMA)
[VMA archive restore outside of Proxmox | Page 3 | Proxmox Support Forum](https://forum.proxmox.com/threads/vma-archive-restore-outside-of-proxmox.14226/post-443929)
### Tags
#tools_Prox 