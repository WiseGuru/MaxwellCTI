---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/tasklist/"}
---

#### tasklist
- `tasklist` shows all tasks running on the computer, in this moment, and their PID
	- Can be run locally or remotely
- One problem is that all `svchosts.exe` appear identical, and can be easy to gloss over

### Command
1. `tasklist`
	1. `tasklist /svc`
		1. For each exe running, it lists the associated services
	2. `tasklist /m`
		1. All the DLLs associated with each executable
	3. `tasklist /m /fi "pid eq [pid]`
		1. `/m` - Lists all tasks curring using the given exe/dll name
		2. `/fi` - Apply a pre-configured filter (e.g., by PID, but status, by username, etc.)
	4. `/u domain\user </p password>`
		1. Run `tasklist` as a different user
		2. If the password is not entered in the optional switch, then the user will be prompted after running



# Metadata

### Sources

### Tags
#tools_win 