---
{"dg-publish":true,"permalink":"/definitions-and-topics/stride/","updated":"2024-04-29T16:44:29.000-07:00"}
---

#### STRIDE
- *STRIDE* is a [[Definitions and Topics/Threat Modeling\|Threat Modeling]] mnemonic created in 1999 by software developers at Microsoft to help developers remember/check different ways an attacker could try to misuse their software.
- The 6 principles are common to frameworks like the [[Definitions and Topics/CIA Triad\|CIA Triad]] and [[Definitions and Topics/AAA\|AAA]]
	- Spoofing (Authenticity)
		- You are who you say you are
	- Tampering (Integrity)
		- Data is not modified by unauthorized parties
	- Repudiation (Non-repudiability/honesty)
		- Actions can be verified to only have been performed by a specific individual
		- e.g., Electronic signatures
			- Anyone can screenshot a wet signature and drop it onto a PDF
			- if they use a certificate-based signature, then only someone with access to the certificate could have signed it
	- Information disclosure (Confidentiality)
		- Sensitive data is not accessible by people who are not authorized to access it
	- Denial of service (Availability)
	- Elevation of Privilege (Authorization)
		- e.g., Privilege Escalation; getting access to accounts you should not have access to.




# Metadata

### Sources
[STRIDE model - Wikipedia](https://en.wikipedia.org/wiki/STRIDE_model)

### Tags
#defs_sec 