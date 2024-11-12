---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/lsblk/"}
---

#### lsblk
- Run `lsblk` to get a list of all storage devices and their partitions
- To get all disks and partitions, with helpful information to identify drives *besides* their `sdX` IDs, use [[Tool Deep-Dives/Linux/grep\|grep]] or [[Tool Deep-Dives/Linux/sed\|sed]] to search for "disk" and "part"
	- `lsblk -o NAME,UUID,PARTUUID,TYPE,PATH,SERIAL | grep 'disk\|part'`
	- `lsblk -o NAME,UUID,PARTUUID,TYPE,PATH,SERIAL | sed -n '1p;/disk\|part/p'`






# Metadata

### Sources
[lsblk(8) - Linux manual page](https://man7.org/linux/man-pages/man8/lsblk.8.html)
### Tags
#tools_linux 