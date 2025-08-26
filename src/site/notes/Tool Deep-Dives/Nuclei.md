---
{"dg-publish":true,"permalink":"/tool-deep-dives/nuclei/","updated":"2024-08-01T11:49:59.915-07:00"}
---

#### Nuclei
- Ultra-flexible vulnerability scanner (or anything scanner)
	- Very flexible, based on templates
	- Uses [[YAML\|YAML]] templates to conduct scans
	- Low to zero false positives (though NOT low to zero false negatives)
- When conducting a pen-test:
	- Automate what you can
		- Use existing automation (e.g., TLS)
		- Make your own automation tools as you go
	- Do the rest by hand
		- Maybe even hire out manual work
	- Highly valuable to have custom automation tools
		- Problems sometimes come back; scan for things you've already seen
- Nuclei is kind of like [[Tool Deep-Dives/Nikto\|Nikto]], but the next step/evolution
	- Both are scanners that look for specific observable conditions (e.g., misconfigurations) on web servers
	- Both based on templates
		- Send *this*, look for *that*
	- Both are easily extended
	- *Key differences*
		- Nuclei uses complex matching, and one-rule/many requests
- Templates
	- *Nuclei Templates* are *complete descriptions for the issue you're looking for*
		- Search for highly specific information, returns great information
	- Self-documenting; you customize the output to provide additional information
		- Check multiple locations and places for information
		- Precisely define what the response should look like



# Metadata

### Sources
[GitHub - projectdiscovery/nuclei: Fast and customizable vulnerability scanner based on simple YAML based DSL.](https://github.com/projectdiscovery/nuclei)
[GitHub - projectdiscovery/nuclei-templates: Community curated list of templates for the nuclei engine to find security vulnerabilities.](https://github.com/projectdiscovery/nuclei-templates)

### Tags
#tools_sec 