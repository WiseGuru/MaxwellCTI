---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/topics/socc-04-server-log-analysis/","updated":"2024-03-11T12:51:02.000-07:00"}
---

# Server Analysis
1. Enable logging and be sure you have a way to analyze them
	1. It's important to run attacks against your own servers to make sure the correct logs are being generated
2. [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks)
	1. Secure configuration guidelines for tons of devices
3. Linux
	1. Linux will condense logs to help prevent log overflow(example below)
		1. `Failed password for root from x port y`
		2. `message repeated 8 more times [Failed password for root from x port y]`
	2. HOWEVER, some SIEMs don't record it correctly
4. **Read the Farking Manual**
	1. *Reading the manual will make you a super hero*
		1. Most people do second-hand learning only, not primary
		2. If you take the time to analyze the RAW^[Rules As Written], your analysis won't be derivative
	2. Identify what's important
		1. What are the key configs?
			1. Files, Tables, GUI
		2. What are the key processes?
			1. Ping, Port, Parse
		3. Where does it store users?
			1. Files, tables, GUI
		4. What are the core ports to be open?
		5. Where are the logs stored or sent?


### Lab: [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-FirewallLog\|Firewall Logs]]
