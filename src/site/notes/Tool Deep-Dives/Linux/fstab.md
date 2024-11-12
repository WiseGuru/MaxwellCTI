---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/fstab/"}
---

#### fstab
- `/etc/fstab` is a file that contains information about file systems.
- File systems can be added by appending the file with a line in the format `[device ID] [mount path] [filesystem] [mount options] [dump frequency] [filesystem checks]`
	- Device ID
		- Anything used to uniquely identify the drive. `/dev/sdX` can be used, but it's strongly recommended to use `/dev/disk/by-*/[file identifier]` instead, with the appropriate information filled out.
		- [[Tool Deep-Dives/Linux/lsblk\|lsblk]] can be used to quickly gather the information needed
	- Mount path
		- This is where the file system will be mounted; typically `/mnt/folder-of-choice`
			- You should create the directory first
	- Filesystem
		- The format of the filesystem being mounted, e.g. ext4, ntfs, etc.
	- Mount Options
		- Various options for more advanced use cases, like preventing it from being mounted with `mount -a`.
		- `defaults` will suffice for most people, and it is customary to include it in the command.
	- Dump Frequency
		- Used by [[dump\|dump]] to determine which filesystems need to be dumped.
			- Defaults to 0
		- Again, highly irregular to configure; use `0` to say don't dump this file system.
	- Filesystem checks
		- Used by [[fsck\|fsck]] to determine the order of filechecks performed
			- **1**: The root filesystem, performed first
			- **2**: Any other filesystem that should be checked
			- **0**: Default, no checks performed.
- Example command: `/dev/disk/by-uuid/123 /mnt/dir-name-of-choice ext4 defaults`
	- Omitting the trailing 0s because they are the defaults.






# Metadata

### Sources
[fstab(5) - Linux manual page](https://man7.org/linux/man-pages/man5/fstab.5.html)

### Tags
#tools_linux 