---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/topics/socc-07-user-and-entity-behavior-analytics/","noteIcon":""}
---



<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/ueba/#ueba" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### UEBA
- *User and Entity Behavior Analytics* (*UEBA*) aggregates logs and analyses behavior.
- Works by assigning values to events, and sending alerts once a certain threshold is reached
	- Example config:
		- Successful user login is +1
		- User logoff is -1
		- Unsuccessful user login is +2
		- Threshold is 6
			- If a user logs in 6 times successfully or fails to login 3 times, an alert is generated
- Requires significant effort to tune and configure a [[Definitions and Topics/baselining\|baseline]]
	- Sometimes has a bad-rap with sysadmins and defenders because it can be so effortful
		- Not plug-and-play






</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/baselining/#baselining" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### baselining
- *Baselining* is the assessment of a system or network to establish a "normal use" definition
	- Necessary when looking for anomalies and [[Definitions and Topics/UEBA\|UEBA]]
- The amount of time it takes to establish a baseline depends on the size and complexity of an organization
	- More time reduces the number of false positives, but if you're already compromised or compromised during the baseline, could lead to false negatives.







</div></div>


## Log Analysis
![giphy.gif](/img/user/Attachments/giphy.gif)
1. Logs are critical in understanding how users behave
	1. Logs can be found on hosts, network devices, servers, services, etc.
2. [[Tool Deep-Dives/LogonTracer\|LogonTracer]]
	1. AD integrated
	2. Track logins across systems and networks
3. Important Event IDs
	1. [Windows Security Log Encyclopedia](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx) is a great resource for understanding Windows Events
	2. Log on and log off events
		1. **4624** Logon
		2. **4634** Logoff
	3. ACL’d object access - Audit requirement
		1. **4662** - An operation was performed on an object
	4. Process launch and usage
		1. **4688** - A new process has been created
	5. Tasks
		1. **4698** - A scheduled task was created
		2. **4702** - A scheduled task was updated
	6. Acct Lockout + Source IP
		1. **4770** - A user account was locked out
		2. **4625** - An account failed to log on
	7. Firewall (noisy connections)
		1. **5152** - The Windows Filtering Platform has blocked a packet
		2. **5154** - The Windows Filtering Platform has permitted an application or service to listen on a port for incoming connections
		3. **5156** - The Windows Filtering Platform has permitted a connection
		4. **5157** - The Windows Filtering Platform has blocked a connection
	9. Special Privileges
		1. **4648** - A logon was attempted using explicit credentials
			1. "Explicit" means not going through the usual authentication process
			2. e.g., Scheduled Tasks or "RUNAS" commands
		2. **4672** - Special Privileges assigned to new logon
		3. **4673** - A privileged service was called
	10. [[Definitions and Topics/Kerberoasting\|Kerberoasting]]
		6. **4769** - A [[Tool Deep-Dives/Kerberos\|Kerberos]] service ticket was requested
		7. **4771** - Kerberos pre-authentication failed
	11. Network Shares
		1. e.g., `\\C$\Users\JohnDoe` or `\\*\IPC$`
		2. **5140** - A network share object was accessed
	13. And many more...
4. How many logs should you gather?
	1. Vendors often say "log everything"
		1. While not wrong, logs are only useful in *preventing* or *responding* to an attack *if you can analyze them*
		2. "Logging everything" without the ability to analyze them is only helpful in post-event reconstruction
	2. In my limited experience, I would say "more than you're logging right now."
		1. I was taking the [[Webinars and Training/BlackHills DFIR Foundations Course/BHIS DFIR Foundations\|BHIS DFIR Foundations]] course, and there were two critical things that resonated here:
			1. "**Prevention is ideal, but detection is a must.**" - Dr. Eric Cole
			2. "Make sure that detection strategies cover all data and business process flows." - Derek Banks
5. Command Line logging
	1. Microsoft makes it hard to audit the command line.
		1. Must enable Audit Process Creation auditing
			1. Enable "Include command line in process creation events"
			2. Confirm these settings are not overwritten by basic audit policy settings
		2. etc. etc.
	2. [[Tool Deep-Dives/Windows/Sysmon\|Sysmon]] is awesome.
		1. Easy to install and default config is incredible
		2. Logs are sent to its own subfolder in Microsoft's Event Viewer
		3. [GitHub - SwiftOnSecurity/sysmon-config: Sysmon configuration file template with default high-quality event tracing](https://github.com/SwiftOnSecurity/sysmon-config)

### LAB: [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-Sysmon\|BHIS-SOCC-lab-Sysmon]]