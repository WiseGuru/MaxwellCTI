---
{"dg-publish":true,"permalink":"/definitions-and-topics/subdomain-enumeration/","updated":"2024-05-06T14:39:11.000-07:00"}
---

#### Subdomain Enumeration
- *Subdomain Enumeration* is literally *enumerating subdomains* (e.g. mail.google.com, calendar.google.com, etc.)
- There are a few tools that help with this.
	- [[Tool Deep-Dives/OSINT/crt-sh\|crt-sh]]
		- Dead simple; enter the domain and get a list of all issued certificates on one page
			- Doesn't include all information, but clicking on the ID provides the full certificate
		- Can also be helpful in quickly getting an idea for site development (first time a cert was requested, etc.)
		- Ad
	- [Entrust Certificate Search - Entrust, Inc.](https://ui.ctsearch.entrust.com/ui/ctsearchui)
		- Easy to export to Excel or CSV, but otherwise doesn't provide as much information
			- Non-exhaustive; only goes back a finite amount of time
	- [[Tool Deep-Dives/OSINT/Google Dorking\|Google Dorking]]
		- Use the *site* search function, like `site:*.google.com`
			- You could also 






# Metadata

### Sources

### Tags
#defs_sec 