---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/topics/socc-06-egress-traffic-analysis/","updated":"2025-06-09T08:59:46.743-07:00"}
---


# Egress Traffic Analysis
1. Need for visibility
	1. [[Definitions and Topics/DLP\|DLP (Data Loss Prevention)]] tools are great, but they're only helpful where they are applied
	2. Attackers bypass DLP/etc., so we need overlapping visibility
		1. Like Guarding the front door while leaving a window open
2. Things to looks for
	1. Outliers and anomalous activity
		1. What only happens on one machine?
	2. Beacons
		1. Tons of short connections from a single client to a remote server
	3. **Long Tail**
		1. On a histogram of connection events, there is a single column that stands out far above the rest
3. Traffic Monitoring Tools and [[Definitions and Topics/NDR\|NDR]]-adjacent solutions
	1. Network Detection and Response (*NDR*) tools are designed to analyze live traffic, detect anomalies, and respond to threats
		1. While there aren't many FOSS tools that are strictly NDR, tools like SIEMs, NIDS/IPS, and XDR have overlapping features
	2. [[Netflow\|Netflow]]
		1. Created by Cisco
		2. Quickly became a standard, but spawned a ton of other variations that are slightly different
	3. [[Zeek\|Zeek]]
		1. Fast
		2. Large user base, lots of support, free
		3. Consistency
	4. [[Tool Deep-Dives/RITA\|RITA]] - Real Intelligence Threat Analytics
		1. Free
		2. Finds patterns in network traffic
		3. Looks for beacons
		4. Analyzing traffic
		5. It aggregates connections together and identifies unusual activity
			1. e.g. IP X beaconed to IP Y X number of times
	5. [Corelight: Evidence-Based NDR and Threat Hunting Platform](https://corelight.com/)
		1. Home Sense is free [Corelight@Home: Who’s Your Fridge Talking to at Night? | Corelight](https://corelight.com/blog/corelight-at-home)
	6. [[Tool Deep-Dives/SOS/Setting up a Security Onion Homelab\|Security Onion]]^[[Security Onion Solutions](https://securityonionsolutions.com/)]
		1. When I tried to deploy this at home, I ran into problems because my hypervisor had a faulty NIC and I didn't have enough ports; I need to try again
	7. [[Tool Deep-Dives/Wazuh/Wazuh Installation\|Wazuh]]^[[Wazuh - Open Source XDR. Open Source SIEM.](https://wazuh.com/)]
		1. More of an EDR solution than a SIEM according to John Strand




### LAB: [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-RITA-ACHunter-01\|BHIS-SOCC-lab-RITA-ACHunter-01]]
