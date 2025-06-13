---
{"dg-publish":true,"permalink":"/tool-deep-dives/metasploit/msfvenom/"}
---

#### msfvenom
- *Msfvenom* is the combination of payload generation and encoding. It replaced msfpayload and msfencode on June 8th 2015.^[[How to use msfvenom | Metasploit Documentation Penetration Testing Software, Pen Testing Security](https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-msfvenom.html)]
- It's a dirty-simple way to generate a Windows executable, custom-made to your needs and specifications.

### Command breakdown
- `msfvenom`
	- `-a` - Architecture
		- You can get the full list of architectures with `--list archs`
	- `--platform` - Target OS
		- Huge list, including Windows, Cisco, PHP, etc.
		- You can get the full list of available formats with `msfvenom --list platforms'
	- `-p` - Payload
		- Specify the payload you're weaponixing
	- `-f` - Format
		- You can get the full list of available formats with `msfvenom --list formats`
	- `-o` - Output
		- Where's the final file going and what's its name?



# Metadata

### Sources
[How to use msfvenom | Metasploit Documentation Penetration Testing Software, Pen Testing Security](https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-msfvenom.html)
[Metasploit Unleashed | MSFvenom | OffSec](https://www.offsec.com/metasploit-unleashed/msfvenom/)
[MSF-Venom-Cheatsheet/MSF Venom Cheatsheet.pdf at master · frizb/MSF-Venom-Cheatsheet · GitHub](https://github.com/frizb/MSF-Venom-Cheatsheet/blob/master/MSF%20Venom%20Cheatsheet.pdf)

### Tags
#tools_metasploit 