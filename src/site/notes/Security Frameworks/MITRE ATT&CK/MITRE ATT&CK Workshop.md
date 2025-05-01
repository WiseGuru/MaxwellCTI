---
{"dg-publish":true,"permalink":"/security-frameworks/mitre-att-and-ck/mitre-att-and-ck-workshop/","noteIcon":""}
---

[Workshop: MITRE ATT&CK Fundamentals - YouTube](https://www.youtube.com/watch?v=1cCt2XZr2ms)

1. Understanding Pyramid of pain
	1. Pyramid that identifies how difficult it is for an attacker to adapt to a defender's response
	2. ![MITRE ATT&CK Workshop-1.png](/img/user/Attachments/MITRE%20ATT&CK%20Workshop-1.png)
		1. The things we traditionally target (hashes, IPs domains, etc.) are easy or trivial for attackers to modify, but as you target higher-level elements, they become harder and harder for an attacker to adapt
			1. e.g., block a virus hash on your network, and the attacker just has to change a single bit for it to skate past the filter
			2. e.g, block a particular tool (PowerShell, etc.), and the attacker will have to figure out an entirely new vector
	3. **TTPs** can be further broken down (Top down, most difficult to adapt, least difficult to adapt)
		1. Tactics
		2. Techniques
		3. Sub-Techniques
		4. Procedures
3. ATT&CK
	1. Across the top are tactics, and each tactic contains the techniques and sub-techniques
		1. Each Technique/sub-technique also includes the mitigations, data sources and detections, and procedure examples
	2. Tactics are pretty static, but techniques change
	3. ![MITRE ATT&CK Workshop-2.png](/img/user/Attachments/MITRE%20ATT&CK%20Workshop-2.png)

*I finished watching the video passively; need to finish my notes here.*
*Actually, IIRC the rest of the video was *