---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/lsblk/","noteIcon":""}
---

#### lsblk
- Run `lsblk` to get a list of all storage devices and their partitions
- To get all disks and partitions, with helpful information to identify drives *besides* their `sdX` IDs, use [[Tool Deep-Dives/Linux/grep\|grep]] or [[Tool Deep-Dives/Linux/sed\|sed]] to search for "disk" and "part"
	- `lsblk -o NAME,UUID,PARTUUID,TYPE,PATH,SERIAL | grep 'disk\|part'`
	- `lsblk -o NAME,UUID,PARTUUID,TYPE,PATH,SERIAL | sed -n '1p;/disk\|part/p'`

## List of options for -o
After 30 seconds of searching for a complete list of variables you can pick from, I gave up and asked ChatGPT to create the list below, so keep that in mind if something doesn't work.

### Generally Useful

1. **NAME**: Device name.
2. **KNAME**: Internal kernel device name.
3. **PATH**: Path to the device node.
4. **MAJ:MIN**: Major and minor device number.
5. **FSAVAIL**: Filesystem size available.
6. **FSSIZE**: Filesystem size.
7. **FSTYPE**: Filesystem type.
8. **FSUSED**: Filesystem size used.
9. **FSUSE%**: Filesystem use percentage.
10. **FSVER**: Filesystem version.
11. **UUID**: Filesystem UUID.
12. **PARTFLAGS**: Partition flags.
13. **PARTLABEL**: Partition label.
14. **PARTTYPE**: Partition type code or UUID.
15. **PARTUUID**: Partition UUID.
16. **PKNAME**: Internal parent kernel device name.
17. **PTTYPE**: Partition table type.
18. **PTUUID**: Partition table identifier (usually UUID).
19. **LABEL**: Filesystem label.
20. **SIZE**: Size of the device.
21. **STATE**: State of the device.
22. **OWNER**: Owner of the device.
23. **GROUP**: Group name.
24. **MODE**: Device node permissions.
25. **TYPE**: Device type (e.g., disk, partition).
26. **MOUNTPOINT**: Where the device is mounted.
27. **MOUNTPOINTS**: All locations where device is mounted.

### Specialty Items

1. **ALIGNMENT**: Alignment offset.
2. **DISC-ALN**: Discard alignment offset.
3. **DAX**: DAX-capable device.
4. **DISC-GRAN**: Discard granularity.
5. **DISC-MAX**: Discard max bytes.
6. **DISC-ZERO**: Discard zeroes data.
7. **HCTL**: Stands for Host:Channel:Target
	1. for SCSI.
8. **HOTPLUG**: Removable or hotplug device status.
9. **LOG-SEC**: Logical sector size.
10. **MIN-IO**: Minimum I/O size.
11. **MODEL**: Device identifier.
12. **OPT-IO**: Optimal I/O size.
13. **RA**: Read-ahead of the device.
14. **RAND**: Adds randomness.
15. **REV**: Device revision.
16. **RM**: Removable device.
17. **RO**: Read-only status.
18. **ROTA**: Rotational device (1 if rotational).
19. **RQ-SIZE**: Request queue size.
20. **SCHED**: I/O scheduler name.
21. **SERIAL**: Disk serial number.
22. **START**: Partition start offset.
23. **SUBSYSTEMS**: Chain of subsystems.
24. **TRAN**: Device transport type.
25. **VENDOR**: Device vendor.
26. **WSAME**: Write same max bytes.
27. **WWN**: Unique storage identifier.
28. **ZONED**: Zoned device type.
29. **ZONE-SZ**: Zone size.
30. **ZONE-WGRAN**: Zone write granularity.
31. **ZONE-APP**: Zone append max bytes.
32. **ZONE-NR**: Number of zones.
33. **ZONE-OMAX**: Maximum number of open zones.
34. **ZONE-AMAX**: Maximum number of active zones.






# Metadata

### Sources
[lsblk(8) - Linux manual page](https://man7.org/linux/man-pages/man8/lsblk.8.html)
### Tags
#tools_linux 