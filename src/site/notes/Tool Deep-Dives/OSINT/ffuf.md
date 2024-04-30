---
{"dg-publish":true,"permalink":"/tool-deep-dives/osint/ffuf/"}
---

#### fuff - Fuzz Faster U Fool
- "A fast web fuzzer written in Go."^[[GitHub - ffuf/ffuf: Fast web fuzzer written in Go](https://github.com/ffuf/ffuf)]
- Input a wordlist, and it searches a website for any pages with corresponding names
	- `ffuf -w /path/to/wordlist -u https://target/FUZZ`
	- Output lists the directory, HTTP response status, size, etc.
- Similar to [[Tool Deep-Dives/OSINT/dirb\|dirb]] and [[Tool Deep-Dives/OSINT/Gobuster\|Gobuster]]





# Metadata

### Sources

### Tags
#tools_osint 