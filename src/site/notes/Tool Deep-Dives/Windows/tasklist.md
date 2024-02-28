---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/tasklist/"}
---

#### tasklist
- `tasklist` shows all tasks running on the computer, in this moment, and their PID
	- Can be run locally or remotely

### Command
1. `tasklist`
	1. Problem is all the `svchosts.exe` that are identical and don't give a shit
	2. `tasklist /svc`
		1. For each exe running, it lists the associated services
	3. `tasklist /m`
		1. All the DLLs associated with each executable
	4. `tasklist /m /fi "pid eq [pid]`
		1. `/m` - Lists all tasks curring using the given exe/dll name
		2. `/fi` - Apply a pre-configured filter (e.g., by PID, but status, by username, etc.)
	5. `/u domain\user </p password>`
		1. Run `tasklist` as a different user
		2. If the password is not entered in the optional switch, then the user will be prompted after running



# Metadata

### Sources

### Tags
#tools_win 