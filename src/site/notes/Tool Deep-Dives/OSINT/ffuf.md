---
{"dg-publish":true,"permalink":"/tool-deep-dives/osint/ffuf/"}
---

#### fuff - Fuzz Faster U Fool
- "A fast web fuzzer written in Go."^[[GitHub - ffuf/ffuf: Fast web fuzzer written in Go](https://github.com/ffuf/ffuf)]
- Input a wordlist, and it searches a website for any pages with corresponding names
	- `ffuf -w /path/to/wordlist -u https://target/FUZZ`
		- `-w` - identifies the wordlist
		- *FUZZ* identifies which part of the path is being fuzzed
	- Output lists the directory, HTTP response status, size, etc.
- Similar to [[Tool Deep-Dives/OSINT/dirb\|dirb]] and [[Tool Deep-Dives/OSINT/Gobuster\|Gobuster]]
- Can be used for subdomain enumeration with the following:
	- First, ding the server with a known bad subdomain to get a known-bad response^[This is important because all responses should "succeed" with a code 200, but the subdomain may not exist.]
		- Could also run the whole list, but might raise flags
		- `ffuf -w <path to namelist> -H "Host: FUZZ.domain.tld" -u <server address or IP>`
			- `-H` - the header filed that's being added or edited
			- `-u` - the target URL (e.g., the server we're pinging)
	- Once you know the size of a known-bad response, 
		- `ffuf -w <path to namelist> -H "Host: FUZZ.domain.tld" -u <server address or IP> -fs <known bad response size>`
			- `-fs` - Filter Size, which tells ffuf to ignore any results of a specific size





# Metadata

### Sources

### Tags
#tools_osint 