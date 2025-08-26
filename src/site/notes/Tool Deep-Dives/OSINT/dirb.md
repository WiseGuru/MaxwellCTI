---
{"dg-publish":true,"permalink":"/tool-deep-dives/osint/dirb/","updated":"2024-04-30T13:59:01.000-07:00"}
---

#### dirb
- *Dirb* (also *dirb* and *DIRB*) is a *Web Content Scanner* that is installed by default on [[Kali Linux\|Kali Linux]]
- Input a wordlist and it searches the site
	- `dirb http://192.168.1.224/ /usr/share/wordlists/dirb/common.txt`
	- Output lists the full URL, HTTP response status, size, etc.
- Slower than [[Tool Deep-Dives/OSINT/ffuf\|ffuf]] and [[Tool Deep-Dives/OSINT/Gobuster\|Gobuster]], but otherwise similar functionality






# Metadata

### Sources
[dirb | Kali Linux Tools](https://www.kali.org/tools/dirb/)
### Tags
#tools_osint 