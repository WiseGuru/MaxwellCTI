---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/path-variable/"}
---

#### $PATH
- Normally to run an application, you need to fully specify the directory path to the application
	- For example, `/home/user1/Downloads/installer.deb`
- `$PATH` is the variable which defines the directories in which the OS looks for applications to run
	- This way, you can just run applications by their name, such as `nuclei` instead of `/home/max/go/bin/nuclei` 
- Which directories are in *PATH*?
	- Just run `echo $PATH` and you'll see all the directories configured
	- To find out where an application is being run *from*, you can use `which [command]`
		- e.g., `which cut` reveals that it's being run from `/usr/bin/cut`
		- ![PATH Variable.png](/img/user/Attachments/PATH%20Variable.png)
- How do you add directories to PATH?
	- Open the config file for your shell of choice
		- For example, `nano ~/.zshrc` or `nano ~/.bashrc`
	- At the very end of the file, add `export PATH="$PATH:path/to/directory/here"`, replacing `path/to/directory/here` with the target directory
		- For example to add the [[Tool Deep-Dives/Golang\|Go]] bin to the PATH variable, I entered `export PATH="$PATH:home/max/go/bin"`
	- Exit Nano and save the file, then Reload your terminal with the new config using `source ~/.zshrc` or `source ~/.bashrc` 





# Metadata

### Sources
[An Introduction to Linux Directories and the PATH Variable](https://blog.ronin.cloud/linux-directories-paths/)
[Getting Started with ProjectDiscovery in Linux and Windows](https://blog.projectdiscovery.io/getting-started-with-projectdiscovery-in-linux-and-windows/)
### Tags
#tools_linux 