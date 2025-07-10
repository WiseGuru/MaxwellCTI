---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/chmod/"}
---

#### chmod
*"change file mode bits"* - [chmod(1) - Linux man page](https://linux.die.net/man/1/chmod)
- `chmod` is a Linux utility to broadly manage folder permissions.
	- Permissions are managed by summing *four octal digits* representing specific levels of access, with the final number representing the permissions for the entity
		- 0 = none
		- 1 = execute (x)
		- 2 = write (w)
		- 4 = read (r)
	- The position of the summed octal digit represents dictates who is getting the permission
		- First position: The object **u**ser (owner)
		- Second position: The **g**roup
		- Third position: All **o**thers
	- The files "user" and "group" are set with [[Tool Deep-Dives/Linux/chown\|chown]]
		- A file may only have one user and one group assigned to it.
- Permissions can be set all at once or piece-meal
	- `chmod 750 plans.txt` would set the permissions for `plans.txt` to User with RWX, Group with R-X, and Others with no access (---).
	- `chmod o+r plans.txt` grants "all others" Read access to the file `plans.txt`. 
- `chmod` is not as fine-grained as [[Tool Deep-Dives/Linux/setfacl\|setfacl]], which allows the configuration of Access Control Lists (ACLs)


| Octal Digit | Permissions          | 3-Character Display |
| ----------- | -------------------- | ------------------- |
| 7           | read, write, execute | rwx                 |
| 6           | read, write          | rw-                 |
| 5           | read, execute        | r-x                 |
| 4           | read                 | r--                 |
| 3           | write, execute       | -wx                 |
| 2           | write                | -w-                 |
| 1           | execute              | --x                 |
| 0           | (none)               | ---                 |
Source: [chmod](https://gps.uml.edu/tutorials/unix-linux/unix/chmod.htm)


# Metadata

### Sources
[chmod](https://gps.uml.edu/tutorials/unix-linux/unix/chmod.htm)
[chmod(1): change file mode bits - Linux man page](https://linux.die.net/man/1/chmod)
### Tags
#tools_linux 