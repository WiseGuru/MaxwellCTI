---
{"dg-publish":true,"permalink":"/tool-deep-dives/7zip/","noteIcon":""}
---

#### 7zip
- *7zip* is a powerful cross-platform archival tool.
- It's easy to use from the CLI, and follows usage patterns across OSs
	- `7z e "*.zip"`
		- Extracts all files from all ZIP archives to the working directory without wrapping them in their directories
			- e.g., instead of "FilesA.zip" and "FilesB.zip" appearing in new folders "FilesA" and "FilesB", they all just intermingle in the working directory
	- `7z x "*.zip"`
		- Extracts all files from all ZIP archives to the working directory in new directories named for the ZIP folders
			- e.g., archived files in "FilesA.zip" will be in a new "FilesA" folder, "FilesB.zip" in a "FilesB" directory, etc.

##### Linux (Debian)
It's probably pre-installed,^[Or I just installed it ages ago and forgot...] but you can install it from the Debian repo with `sudo apt install p7zip-full`

### Powershell
[[Tool Deep-Dives/Windows/PowerShell\|PowerShell]]'s Verb-Noun format doesn't exactly jive with 7zip's simple instruction set, and normally you have to run the 7zip.exe from the target path. However, it's simple to add it as an variable, then alias, to allow you to use the same commands as normal.

```PowerShell
$7zipPath = "$env:ProgramFiles\7-Zip\7z.exe"
set-alias 7z $7zipPath
```

Here's an example following 
![7zip.png](/img/user/Attachments/7zip.png)

If you want to make this permanent on your profile, you can edit your `profile.ps1` script (which runs whenever *you* launch Powershell) to set the variables for you; be warned though, that this may get you into the false-sense of security that the `7z` command will just work on any other computer you interact with.

To change `profile.ps1`, just run `notepad $profile` from your Powershell terminal, and copy/paste the code above into it. Save the script, restart Powershell, and you should be good to go.


# Metadata

### Sources
[Install and use 7zip on Linux - TutorialsPoint.com](https://www.tutorialspoint.com/install-and-use-7zip-on-linux)
[7za man page - edenwaith.com](http://www.edenwaith.com/support/untar/help/man/7za.html)
[7zip - Running 7-Zip from within a Powershell script - Stack Overflow](https://stackoverflow.com/questions/25287994/running-7-zip-from-within-a-powershell-script#25288780)
[How to create permanent PowerShell Aliases - Stack Overflow](https://stackoverflow.com/a/50954674)
### Tags
#tools_sec 