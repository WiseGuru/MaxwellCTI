---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/installing-software-on-debian/","updated":"2025-06-30T14:20:05.974-07:00"}
---

There are a few different ways to install software on a Debian; this not a complete list, and there are various other tools (like [Nala](https://gitlab.com/volian/nala)) that further extend and modify the native tools.

- `dpkg`
	- Simple package manager and installer for Debian
	- `-i` or `--install` install a given file
		- e.g., `sudo dpkg -i example.deb`
	- Does not install dependencies
- `apt` or `apt-get`
	- Can be used to install packages from repositories/PPAs or locally
		- `sudo apt install ./local/path/to/package.deb`
		- `sudo apt install firefox`
- `gdebi`
	- Termina/GUI wrapper for `dpkg` that also installs depencies
- `qapt-deb-installer`
	- A GUI wrapper for APT that makes it easy to install `.deb` files by double-clicking them (and probably other things)




# Metadata

### Sources
[How to Install a DEB File in Linux? - Scaler Topics](https://www.scaler.com/topics/linux-deb/)
[Debian -- Details of package qapt-deb-installer in sid](https://packages.debian.org/sid/kde/qapt-deb-installer)
### Tags
#tools_linux 