---
{"dg-publish":true,"permalink":"/tool-deep-dives/proxmox/mounting-usb-drives-in-prox-mox/","updated":"2024-11-12T14:47:31.067-08:00"}
---

#### Summary
This guide was built using [syncbricks'](https://www.youtube.com/@syncbricks) video, linked below.
1. Erase the disk
	1. Run [[Tool Deep-Dives/Linux/lsblk\|lsblk]] to identify the device
		1. You can use `lsblk -o NAME,UUID,ID-LINK,LABEL,MODEL,PARTUUID,TYPE,PATH,MOUNTPOINTS | sed -n '1p;/disk\|part/p'` to get the information you need for each drive to get a more consistent method of ID^[[[grep\|[grep]] works too, but [[Tool Deep-Dives/Linux/sed\|sed]] can display the header]
			1. `NAME`: for your own reference, shows parent drives and partitions
			2. `UUID`: `/dev/disk/by-uuid`
			3. `ID-LINK`: `/dev/disk/by-id` (*most helpful*)
			4. `LABEL`: `/dev/disk/by-label` (likely unhelpful, probably should remove)
			5. `MODEL`: for your own reference
			6. `PARTUUID`: `/dev/disk/by-partuuid` (only helpful with partitions)
			7. `TYPE`: for sorting
			8. `PATH`: for your own reference
			9. `MOUNTPOINTS`: for your own reference
	2. Two methods to wipe the drive
		1. `sudo wipefs -a /dev/sdX`, with X being the drive's *NAME*
			1. **Warning**: As [Xepher](https://www.reddit.com/r/linux/comments/9rswgf/comment/e8jhvks/) notes, you're more likely to select the wrong disk with `/dev/sdX`, and the better alternative is to use `/dev/disk/*`, which has a bunch of folders grouped by better identifiers for disks. While a disk `sdX` ID might change between reboots or systems, the parameters identified in `/dev/disk/*` are not.
		2. `sudo wipefs -a /dev/disk/by-label/xyz`, with `xyz` being the drive's *ID-LINK*
			1. Don't forget that you can hit *Tab* once you've written most of the command for it to autofill what must remain.
			2. For example, `sudo wipefs -a /dev/disk/by-label/usb-G`+*Tab* would complete as `sudo wipefs -a /dev/disk/by-label/usb-Generic_STORAGE_DEVICE-0:0` if that was the only label that started with `usb-G`
2. Initialize Disk with GPT and create the partition
	1. Either through the GUI and *Initialize with GPT* or with `sgdisk -N 1 /dev/disk/by-label/xyz`
3. Format the partition
	1. `mkfs.ext4 /dev/disk/by-partuuid/123` with 123 being the PARTUUID
		1. You will have to re-run the `lsblk` command from earlier, but you will use the PARTUUID in the next step
4. Create mount directory and mount partition
	1. Create a mountpoint directory with `mkdir /mnt/dir-name-of-choice` (e.g., `/mnt/usb-drive` or `/mnt/ext-backup`)
	2. Use nano to append [[Tool Deep-Dives/Linux/fstab\|fstab]] to include the PARTUUID and mount point you created
		1. `nano /etc/fstab`
		2. `/dev/disk/by-uuid/123 /mnt/dir-name-of-choice ext4 defaults`,^[Check [[Tool Deep-Dives/Linux/fstab\|fstab]] for details.] with 123 being the PARTUUID
	3. Run `systemctl daemon-reload && mount -a` to reload the system daemons and [[Tool Deep-Dives/Linux/mount\|mount]] all file systems
		1. As a reminder, `&&` allows you to run two commands sequentially in the same line.
	4. Confirm all is correct by running `lsblk` to see that it's mounted
		1. Refreshing the GUI should also show that it's mounted.
5. Add to Datacenter through the GUI
	1. Navigate up a level from your host system to the Datacenter view, go to Storage, and select the "Add" dropdown menu, and choose "Directory"
	2. Fill in the information requested:
		1. ID: any name you want
		2. Directory: the mountpath you configured earlier (`/mnt/dir-name-of-choice)
		3. Content: Select *everything* to make sure you can use the drive however you would like.
	3. The drive should now appear under the host, and allow you to store any information you want there.


# Metadata

### Sources
[Proxmox USB Storage Tutorial - How to mount USB disks in Proxmox VE - YouTube](https://www.youtube.com/watch?v=a3QTaV4Cg7M)
### Tags
#tools_Prox 