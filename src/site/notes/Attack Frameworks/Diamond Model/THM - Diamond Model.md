---
{"dg-publish":true,"permalink":"/attack-frameworks/diamond-model/thm-diamond-model/"}
---

![THM - Diamond Model-1.png](/img/user/Attachments/THM%20-%20Diamond%20Model-1.png)
[The Diamond Model of Intrusion Analysis.pdf](https://www.activeresponse.org/wp-content/uploads/2013/07/diamond.pdf)
1. Diamond Model of Intrusion
	1. A 4-pointed model of Adversary-Victim-Capability-Infrastructure that can be used to analyze any intrusion activity
2. 4-points
	1. Adversaries
		1. Adversary Operator
			1. The hacker/person/people conducting the attack
		2. Adversary Customer
			1. The entity which stands to benefit from the attack
	2. Victims
		1. Victim Personae
			1. The people
		2. Victim Assets
			1. The assets
	3. Capability
		1. All the tools, techniques, and TTPs used by the adversary
		2. Capability capacity
			1. The range of vulnerabilities and exposures that an individual capability (e.g. tool, technique, etc.) can take advantage of, regardless of the victim
		3. Adversary Arsenal
			1. The adversary's complete set of capabilities
	4. Infrastructure
		1. Type 1
			1. Infrastructure owned and used by the adversary
		2. Type 2
			1. Infrastructure owned by an intermediary and used by an adversary
			2. The intermediary may or may not be aware
		3. Service Providers
3. Event Meta Features
	1. Meta-features are not required, but can add valuable information to the model
	2. Six possible "meta-features" are applicable
		1. Timestamp
			1. When specific events occurred
		2. Phase
			1. e.g., which phase of the [[Attack Frameworks/LM Cyber Kill Chain/THM - LM Cyber Kill chain\|Cyber Kill Chain]]
			2. "Every malicious activity contains *two or more phases* which must be *successfully executed in succession* to achieve the desired result"
		3. Result
			1. Not always clear from an attack, but whether certain attempts were failures or successes can be helpful
			2. Or identifying what's uncertain
		4. Direction
			1. 6 potential values between the Adversary, Infrastructure, and Victim (including bidirectional and unknown)
		5. Methodology
			1. General classification of the intrusion event, like phishing, DDoS, etc.
		6. Resources
			1. The resources required to carry out the intrusion event
				1. e.g. Software, knowledge, funds, etc.
4. Social-political component
	1. The needs of the adversary; financial, social, hacktivism, espionage, etc.
