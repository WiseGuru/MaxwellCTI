---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/sed/","updated":"2024-11-12T13:02:22.901-08:00"}
---

#### sed
- *sed* is a stream editor for filtering and transforming text
- It can work similarly to `grep`, but can be configured to include the headers.
	- e.g., `lsblk -o NAME,UUID,PARTUUID,TYPE,PATH,SERIAL | sed -n '1p;/disk\|part\|loop/p'`
		- `-n` suppresses automatic printing of each line.
		- `'...'` allows for queries with multiple parts
		- `1p` means "print the first line"
		- `;` separates the different parts of the query
		- `/.../` contains the search parameters
		- `\|` functions as an "or" statement (like in [[Tool Deep-Dives/Linux/grep\|grep]])
		- `p` prints any line that matches



# Metadata

### Sources

### Tags
#tools_linux 