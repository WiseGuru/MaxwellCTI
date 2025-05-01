---
{"dg-publish":true,"permalink":"/tool-deep-dives/path-traversal/","tags":["Linux"],"noteIcon":""}
---

#### Path Traversal
- A common low-effort technique to explore a Linux is to attempt *path traversal*.
	- This is where you try to look in other folders that you wouldn't normally have access to, typically out of an application and into the system.
- A common method is through by using the `..\` folder shortcut
	- `.` refers to the current directory
	- `..` refers to the parent directory
	- `..\..\..` refers to three levels above the current directory
	- `..\..\..\..\..\etc\passwd` navigates up five levels, and then goes *down* to the `/etc/passwd` folder (which is a [bad-bad not good](https://www.youtube.com/watch?v=H-qmZ_J7WGc))
- Example:
	- A website might allow users to access files on a specific location on its drive
		- e.g., `constoso.com/store/schedules/January.pdf`
	- An attacker could attempt to navigate up the folder structure by injecting those parent-folder shortcuts
		- e.g., `contoso.com/store/schedules/../../../../etc/passwd` or `contoso.com/store/../../../../etc/passwd`






# Metadata

### Sources*