---
{"dg-publish":true,"permalink":"/frameworks-standards-and-regulations/unified-kill-chain/thm-unified-kill-chain/"}
---

#thm 
[TryHackMe | Why Subscribe](https://tryhackme.com/room/unifiedkillchain)

1. Threat modeling
	1. "Threat Modeling" is the process of:
		1. Identifying systems
		2. Assessing vulnerabilities
		3. Create a plan of action to secure those system within acceptable risk
		4. Put policies in place to prevent future vulnerabilities
			1. e.g., patch management, [[Definitions and Topics/SDLC\|SDLC]], awareness training, etc.
2. [The Unified Kill Chain](https://www.unifiedkillchain.com/)
	1. Aims to compliment other cybersecurity frameworks
	2. The goal is to cover an entire attack, and recognizes that an attacker will move between links in a non-linear fashio
		1. The [[Frameworks, Standards, and Regulations/LM Cyber Kill Chain/THM - LM Cyber Kill chain\| LM CKC]] framework really only focuses on malware delivery, and is primarily linear
	3. There are 18 "links" in the kill chain, and they are organized in three groups; In, Through, and Out
		1. In
			1. Details how an attacker attempts to gain and maintain access
		2. Through
			1. How an attacker might move through an organization
		3. Out
			1. How an attacker accomplishes objectives and exits the organization
3. In
	1. Reconnaissance
		1. [MITRE Tactic TA0043](https://attack.mitre.org/tactics/TA0043/)
		2. Attacker aims to gather information about the target
	2. Weaponization
		1. [MITRE Tactic TA0001](https://attack.mitre.org/tactics/TA0001/)
		2. Attacker configures the necessary infrastructure to perform the attack
			1. e.g. C2 Servers, etc.
	3. Social Engineering
		1. [MITRE Tactic TA0001](https://attack.mitre.org/tactics/TA0001/)
		2. Attacker attempts to manipulate employees to perform actions to help in the attack
	4. Exploitation
		1. [MITRE Tactic TA0002](https://attack.mitre.org/tactics/TA0002/)
		2. How an attacker takes advantage of a system vulnerability
	5. Persistence
		1. [MITRE Tactic TA0003](https://attack.mitre.org/tactics/TA0003/)
		2. The techniques used by an adversary to maintain access to a system
	6. Defense Evasion
		1. [MITRE Tactic TA0005](https://attack.mitre.org/tactics/TA0005/)
		2. Basically how an attacker avoids detection and defensive measures
	7. Command & Control
		1. [MITRE Tactic TA0008](https://attack.mitre.org/tactics/TA0011/)
		2. Combines the efforts from *Weaponization* to establish communications between the adversary and target systems
	8. Pivoting
		1. [MITRE Tactic TA0008](https://attack.mitre.org/tactics/TA0011/)
		2. How an adversary moves within a network to access other systems (for example, a fileshare without access to the internet)
4. Through
	1. Pivoting
		1. [MITRE Tactic TA0008](https://attack.mitre.org/tactics/TA0011/)
		2. In the *Through* phase, *Pivoting* might be how an attacker uses a system as a staging and distribution site for malware
	2. Discovery
		1. [MITRE Tactic TA0007](https://attack.mitre.org/tactics/TA0007/)
		2. The adversary gaining information about the systems they have access to; like *Reconnaissance*, but more keyed into the active investigation on target systems
	3. Privilege Escalation
		1. [MITRE Tactic TA0004](https://attack.mitre.org/tactics/TA0004/)
		2. Attempts to gain elevated access (*root*, *Local Administrator*, users with specific access, etc.) on key systems
	4. Execution
		1. [MITRE Tactic TA0002](https://attack.mitre.org/tactics/TA0002/)
		2. An attacker deploys the *Weaponized* code and infrastructure developed earlier.
	5. Credential Access
		1. [MITRE Tactic TA0006](https://attack.mitre.org/tactics/TA0006/)
		2. This works with the *Privilege Escalation* stage, and employs tools for keylogging and credential dumping
	6. Lateral Movement
		1. [MITRE Tactic TA0008](https://attack.mitre.org/tactics/TA0011/)
		2. Kind of a mix of *Pivoting*, *Privilege Escalation*, and *Credential Access* to move across networks and systems to access the targeted systems
5. Out
	1. Collection
		1. [MITRE Tactic TA0009](https://attack.mitre.org/tactics/TA0009/)
		2. After all the prior activities, the adversary analyzes data sources in search of valuable data information to *Exfiltrate* 
	2. Exfiltration
		1. [MITRE Tactic TA0010](https://attack.mitre.org/tactics/TA0010/)
		2. Adversary seeks to steal the data, which would be compressed and encrypted to avoid detection and DLP
		3. The *C2* channel and tunnel is often used in this process
	3. Impact
		1. [MITRE Tactic TA0040](https://attack.mitre.org/tactics/TA0040/)
		2. If the goal is to interrupt normal business or destroy assets, this technique describes those activities
	4. Objectives
		1. This is a broad term that's just "The primary objects the adversary originally sought to complete"



Random shit:
143.110.250.149