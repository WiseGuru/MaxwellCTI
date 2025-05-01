---
{"dg-publish":true,"permalink":"/tool-deep-dives/volatility/","noteIcon":""}
---

#### Volatility
- *Volatility* is a *memory forensics* tool that was designed to work cross-platform with [[Tool Deep-Dives/Linux/Linux\|Linux]], [[Tool Deep-Dives/Windows/Windows#Getting Better Memory Dumps\|Windows]], and [[macOS\|macOS]]
	- Basically any platform that supports [[Tool Deep-Dives/Python/Python\|Python]] should support Volatility
	- It's available both in CLI and in GUI with the [Volatility Workbench](https://www.osforensics.com/tools/volatility-workbench.html)
- *Volatility* can be modified with a range of modules and plugins, extending its functionality.
	- For example, here's a list of all available plugins to analyze Windows OS memory files.
	- [volatility3.plugins.windows package — Volatility 3 2.0.1 documentation](https://volatility3.readthedocs.io/en/v2.0.1/volatility3.plugins.windows.html)
- Per my tests, it does not appear to really take advantage of multithreaded processing and can take several minutes to parse a memdump file.
	- ![Volatility.png](/img/user/Attachments/Volatility.png)
	- Therefore, maybe run multiple instances at once to get different pieces of information
	- ![Volatility-1.png](/img/user/Attachments/Volatility-1.png)

### Important Modules
Most of these modules were explored in the [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-MemoryForensics\|BHIS SOCC course Memory Forensics lab]] I took earlier, but that was using a much earlier version of Volatility; here are additional references and links for the latest version of Volatility.
- [netscan](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#netscan)
- [pslist](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#pslist)
	- Show most processes, just not the hidden or unlinked ones
- [pstree](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#pstree)
	- Like pslist, but visually shows the relationship between processes
- [psscan](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#psscan)
	- Searches ALL processes, included hidden or unlinked processes
- [dlllist](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference#dlllist)
	- Used to display the DLLs loaded by processes
	- Can be refined to specific DLLs with `--pid <PID>`
	- Displays DLLs hidden/injected via `CreateRemoteThread->LoadLibrary`
- [malfind](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference-Mal#malfind)
	- Identifies hidden/injected code or DLLs in user memory, based on the [[Definitions and Topics/VAD\|VAD]] tag and permissions
	- It *does not* show DLLs hidden/injected via `CreateRemoteThread->LoadLibrary` because that's frankly pushing into dlllist's territory and it should stay in its lane.
	- The `VPN` is *Virtual Page Numbers*, and is basically where the code begins and ends in memory

### Volatility Commands
- `python vol.py -h`
	- Get help on the Volatility tool, displaying all switches etc.
- `python vol.py -f <file name>`
	- Identify the file being investigated






# Metadata

### Sources
[Volatility Foundation · GitHub](https://github.com/volatilityfoundation)
[The Volatility Foundation](https://volatilityfoundation.org/)
[Volatility 3 CheatSheet - onfvpBlog](https://blog.onfvp.com/post/volatility-cheatsheet/)
### Tags
#tools_sec
