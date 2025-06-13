---
{"dg-publish":true,"permalink":"/tool-deep-dives/whois/"}
---

#### whois
- `whois` is a command-line utility that retrieves registration about IPs, domain names, and network devices registered with [ICANN](https://www.icann.org/)
- Utility exists for Linux, Windows, and macOS
	- Native on macOS, must be installed on Windows, and depends on the Linux distribution
- Use is straightforward, though there may be some variations between OS versions
	- `whois [options] [object]`
		- `[options]` include choosing the specific registrant server to query, hiding disclaimers, showing only abuse contacts, etc.
		- `[object]` the IP address or domain name being queried.
- When paired with [[Tool Deep-Dives/Linux/grep\|grep]], it can make retrieving certain information very efficient.

![whois.png](/img/user/Attachments/whois.png)
> The start of the response from a [[Tool Deep-Dives/whois\|whois]] command

![whois-1.png](/img/user/Attachments/whois-1.png)
> [[Tool Deep-Dives/whois\|whois]] paired with [[Tool Deep-Dives/Linux/grep\|grep]]

# Metadata

### Sources
[Whois - Sysinternals \| Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/whois)
[How to install whois on Ubuntu / Debian Linux - nixCraft](https://www.cyberciti.biz/faq/how-to-install-whois-on-ubuntu-debian-linux/)
[How to use the whois command on Ubuntu Linux \| GeeksforGeeks](https://www.geeksforgeeks.org/how-to-use-the-whois-command-on-ubuntu-linux/)
### Tags
#tools_sec 