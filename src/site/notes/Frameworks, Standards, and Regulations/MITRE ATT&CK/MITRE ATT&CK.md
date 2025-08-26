---
{"dg-publish":true,"permalink":"/frameworks-standards-and-regulations/mitre-att-and-ck/mitre-att-and-ck/","updated":"2024-03-05T20:39:08.000-08:00"}
---

## MITRE ATT&CK Overview

### [MITRE ATT&CK®](https://attack.mitre.org/)

1. Front-Page Overview
	1. Across the top are Tactics
		1. The overall goals/objectives the adversary is attempting to carry out
	2. Below each Tactic are various Techniques
		1. The techniques are *how* the adversary accomplishes the tactic
		2. The same technique can be used across multiple tactics
			1. e.g., the "BITS Jobs"^[Windows Background Intelligent Transfer Service] technique occurs under both Persistence and Defense Evasion  
	3. Many techniques have a range of *sub-techniques* that are more specific ways they can gain access
	4. ![ATT&CK Resources.png](/img/user/Attachments/ATT&CK%20Resources.png)
2. Navigation
	1. Everything is assigned a unique ID built from the kind of thing it is followed by a 4-digit number
		1. *Tactics* use *TA*
		2. *Techniques* use *T*
		3. *Software* use *S*
		4. *Campaigns* use *C*
		5. *Groups* use *G*
		6. *Mitigations* use *M*
		7. *Data Sources* use *DS*
		8. *Assets* (specific to ICS systems) use *A*
3. Techniques
	1. Each Technique is assigned a unique T-number, and sub-techniques are assigned a .xyz decimal from its main technique
		1. For example, Brute Force is T1110, and its sub-technique Password Guessing is T1110.001
	2. Each Technique has 2 or 3 sections
		1. Procedure Examples
4. Groups
	1. Groups are rough associations of TTPs to individuals or associations

### [ATT&CK® Navigator](https://mitre-attack.github.io/attack-navigator/)
1. GitHub app that allows you to build a personalized framework for your purposes
	1. Organizational strengths, APT common techniques, etc.
2. Search and Filter
	1. Click on the Magnifier to select all the techniques used by a particular APT, or Software, or bang-for-buck mitigations
		1. Then select the paint can to fill those selected cells with a different color and assign a numerical value
		2. It's best to create separate layers for each modified search
	2. When you select techniques, color-code them and assign a score
	3. Once all layers created, create a new layer based on layers, and use math operations (a + b + c) to combine them
3. Navigator maps can be exported as JSONs, and imported too
	1. Other vendors have mapped their tools to *Navigator,* like [Red Canary's Atomic Red Team](https://github.com/redcanaryco/atomic-red-team), where you can import its capabilities into a layer (pre-color coded and scored) in Navigator