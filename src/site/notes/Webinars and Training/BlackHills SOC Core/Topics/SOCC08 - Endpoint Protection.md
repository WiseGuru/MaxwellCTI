---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/topics/socc-08-endpoint-protection/"}
---

**NOTE**: We kinda rushed through this section in the class and I'm a little under the weather as I'm working on it now, so I've tidied it up a bit, but want to fill it in more later.
# Endpoint Protection Analysis
## Endpoint detection and testing tools
1. [[Tool Deep-Dives/BLUESPAWN\|BLUESPAWN]] runs and checks the system for all kinds of issues
	1. Detections are marked in MITRE Techniques
2. [[Tool Deep-Dives/Atomic Red Team\|Atomic Red Team]] for testing an EDR
	1. **Do not run ART in production**

## Free or Good EDRs
1. [[Tool Deep-Dives/Wazuh/Wazuh\|Wazuh]]
	1. Originally an inventory system
	2. Full stack EDR
	3. Data feeds to an ELK stack
2. [[Tool Deep-Dives/Velociraptor\|Velociraptor]]
3. LimaCharlie
4. [[Tool Deep-Dives/Elastic Stack\|Elastic Stack]]
	1. Almost everyone uses ELK
		1. ELK is free, Elastic is paid
	2. Easy install
	3. Can install it and start detecting malware out of the box
5. [[Tool Deep-Dives/OpenEDR\|OpenEDR]]
	1. From Comodo
	2. Full source code on GitHub


### LAB: [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-Velociraptor\|BHIS-SOCC-lab-Velociraptor]]
