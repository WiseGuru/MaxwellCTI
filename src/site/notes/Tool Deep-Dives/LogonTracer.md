---
{"dg-publish":true,"permalink":"/tool-deep-dives/logon-tracer/"}
---

#### LogonTracer
- *LogonTracer* visualizes Windows [[Tool Deep-Dives/Windows/Active Directory\|AD]] event logs to assist investigating malicious activity
	- It looks for these specific event IDs:
		- **4624**: Successful logon
		- **4625**: Logon failure
		- **4768**: [[Tool Deep-Dives/Kerberos\|Kerberos]] Authentication (TGT Request)
		- **4769**: Kerberos Service Ticket (ST Request)
		- **4776**: NTLM Authentication
		- **4672**: Assign special privileges






# Metadata

### Sources
[GitHub - JPCERTCC/LogonTracer: Investigate malicious Windows logon by visualizing and analyzing Windows event log](https://github.com/JPCERTCC/LogonTracer)
### Tags
#tools_sec 