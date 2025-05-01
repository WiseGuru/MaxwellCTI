---
{"dg-publish":true,"permalink":"/security-frameworks/lm-cyber-kill-chain/thm-lm-cyber-kill-chain/","tags":["thm"],"noteIcon":""}
---

1. Intro
	1. There are 7 links in the kill chain
		1. Reconnaissance
		2. Weaponization
		3. Delivery
		4. Exploitation
		5. Installation
		6. Command and Control
		7. Actions on Objectives
	2. The Cyber Kill Chain helps understand process of steps and layers of an attack
2. Reconnaissance
	1. Discovery and collection of information related to the target.
	2. [[Definitions and Topics/OSINT\|OSINT]] - Open-Source Intelligence
		1. The first and most subtle step an attacker can take to gather information
		2. Study of publicly available information
			1. Never makes contact with the target, so it's very difficult to detect
	3. [[Definitions and Topics/Email harvesting\|Email harvesting]] and phishing
3. Weaponization
	1. A "weaponizer" combines an exploit and malware into a deliverable payload
		1. The *exploit* is a program or code used to take advantage of a *vulnerability* in a system
			1. As an example, it is an empty wooden horse, or a word document with macros enabled
		2. The *malware* is the code or program that is intended to take action on a system (e.g. disrupt, gain access, damage, etc.)
			1. In the example, it's a group of soldiers, or a macro that executes code on a system
		3. The *payload* is the combination of the *exploit* and the *malware* that is delivered to a system
			1. the soldiers in the wooden horse, or the [word document with macros to infect a system](https://www.trustedsec.com/blog/intro-to-macros-and-vba-for-script-kiddies)
	2. IF the goal is more long-term, the attacker may attempt to leave themselves a *backdoor*
4. Delivery
	1. How the *payload* is delivered
		1. Phishing email, infected USB drives, or a watering hole attack, for example
		2. [Cybercriminal group mails malicious USB dongles to targeted companies | CSO Online](https://www.csoonline.com/article/569163/cybercriminal-group-mails-malicious-usb-dongles-to-targeted-companies.html)
5. Exploitation
	1. A vulnerability is exploited, either with the *payload* created earlier, or in addition (e.g., employee clicking a link, etc.)
	2. The exploits can be used to move *laterally* through a system
		1. [[Definitions and Topics/Lateral movement\|Lateral movement]] is when a malicious actor is able to use access in one area of a system to access another area in the system
			1. e.g., using an EA's credentials to log into the CEO's laptop
	3. A [[Definitions and Topics/Zero-day\|Zero-day]] is a vulnerability or exploit is a vulnerability or exploit that is unknown by the developer, and can be exploited on a fully-patched system
6. Installation
	1. Once the attacker has entered the system, they want to install their malware, often to create a *backdoor*
	2. There are various ways to gain *persistent backdoor access* to a system:
		1. [[Web shell\|Web shell]] - a malicious script written in webdev languages like ASP, PHP, or JSP, to maintain access to a compromised system
			1. [Web shell attacks continue to rise | Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2021/02/11/web-shell-attacks-continue-to-rise/)
		2. Install a *backdoor application* that is specifically designed to maintain persistent access on a system
			1. For example, [Meterpreter in Metasploit](https://www.offsec.com/metasploit-unleashed/meterpreter-backdoor/) or even something like Teamviewer
		3. Modifying system Services.
			1. [Create or Modify System Process: Windows Service, Sub-technique T1543.003 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1543/003/)
			2. These malicious services are often disguised by [Masquerading](https://attack.mitre.org/techniques/T1036/) as legitimate or known names for their applications, like Notepad
		4. Adding *run keys* or similar to system startup locations
	3. [[Timestomping\|Timestomping]] allows  is when an attacker modifies file and log timestamps to evade detection
		1. [Indicator Removal: Timestomp, Sub-technique T1070.006 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1070/006/)
7. Command & Control
	1. *C2 (Command and Control)* is how an attacker communicates with a compromised machine and maintains control over its operation
		1. Also known as *C2 or C&C Beaconing*
	2. Historically, *IRC (Internet Relay Chat)* was frequently used as a C2 communication channel, but there are now *2 common methods* to evade firewalls
		1. *HTTP 80 and HTTPS 443* - blends malicious traffic with legitimate traffic, and is difficult to detect
		2. *DNS Tunneling* - frequent DNS requests to a DNS server owned by the attacker, making the traffic appear legitimate.
	3. The *C2* device or infrastructure can also be owned by the attacker (e.g., a raspberry pi)
8. Actions on Objectives (Exfiltration)
	1. The attacker typically now takes its *actions on objectives*, whether that is *corrupting* or *collecting* data. or *going deeper*
		1. Collecting
			1. Credentials, recon of internal software vulnerabilities, sensitive information, etc.
			2. Goal might be to resell information, ransom it, or use it to craft more targeted attacks
		2. Corrupting
			1. Deleting backups, encrypting or corrupting records
			2. Goal might be to ransom information, inflict corporate damage, or conceal information, etc.
		3. Going deeper
			1. Further privilege escalation or lateral movement, like to air-gapped equipment etc.,
			2. Goal is to perform further attacks
9. Conclusion
	1. The Cyber Kill Chain is not to be relied upon
		1. It was last updated in 2011, when it was published
	2. The landscape and our understanding has evolved since then
	3. MITRE and the [[Security Frameworks/Unified Kill Chain/THM - Unified Kill Chain\|Unified Kill Chain]] should be used instead
## Practice
The analysis is on Target's credit card compromise, and we're given a list of possibilities.
- exploit public-facing application
- data from local system
- PowerShell
- dynamic linker hijacking
- spearphishing attachment
- fallback channels

And we're supposed to align these with the 6 stages above Recon
1. Weaponization
	1. I initially thought this would by the *dynamic link hijacker*, as I was thinking of that as the malware
	2. However, the correct answer is *PowerShell*, because PowerShell (or VB script etc.) is used to *weaponize* the exploit and malware in a payload
2. Delivery
	1. Clearly *spearphishing attachment*, since that's how it gets the payload is *delivered*
3. Exploitation
	1. Very likely *exploit public-facing application*, because at this point the attackers were skimming credit cards
4. Installation
	1. Here I thought it was *PowerShell*, because PowerShell = install
	2. However, I need to connect *Installation with Persistence* in my mind
	3. Therefore, [*dynamic linker hijacking*](https://attack.mitre.org/techniques/T1574/006/) is how the attackers maintain *persistence* 
5. Command & Control
	1. Use *fallback channels* for communication
6. Actions on Objectives
	1. **exfiltrate** *data from local system* as their objective (stealing credit card information)

