---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/cut/"}
---

#### cut
- *cut* is a CLI tool that cuts up the output of a script or file
	- `-b` - bytes
	- `-c` - characters
	- `-d` - delimiter
		- Identifies the delimiter used in the input
		- `-d','` identifies commas as delimiters
		- `-d'\t'` or with an actual *tab* character identifies a tab
	- `-f#,#` - identifies the fields that should be extracted
	- `-s` - only prints lines with delimiters






# Metadata

### Sources
[cut(1): remove sections from each line of files - Linux man page](https://linux.die.net/man/1/cut)

### Tags
#tools_linux 