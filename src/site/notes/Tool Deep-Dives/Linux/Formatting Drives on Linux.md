---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/formatting-drives-on-linux/"}
---

#### Summary
When you mount a drive to Linux that you need to wipe, you might run into an error like this:
```bash
sudo wipefs -a /dev/sda
wipefs: error: /dev/sda: probing initialization failed: Device or resource busy
```
This is likely because Linux has a *partition* on that drive mounted, so it cannot make changes to the drive itself.

To solve it: 
1. Run [[Tool Deep-Dives/Linux/lsblk\|lsblk]] to identify any mounted partitions on that drive (e.g.,, `sda1`, `sda2`, etc.)
2. Unmount just the partitions `sudo umount /dev/sda1`
3. Erase the storage, for example with `sudo wipefs -a /dev/sda`

Now you can do whatever you want with the drive, hopefully nothing to raunchy.



# Metadata

### Sources

### Tags
#tools_linux 