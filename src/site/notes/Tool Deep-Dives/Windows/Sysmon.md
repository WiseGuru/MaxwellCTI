---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/sysmon/"}
---

#### Sysmon
- _System Monitor_ (_Sysmon_) is a Windows system service and device driver that, once installed on a system, remains resident across system reboots to monitor and log system activity to the Windows event log.^[[Sysmon - Sysinternals | Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)]
- It is **FREE**, but does **NOT** come pre-installed on Windows
- Super easy to install and use
	- Three steps to install:
		- Download it from [here (from Microsoft): Sysmon.zip]
		- Unzip the file
		- Open PowerShell or CMD, CD to the location, and run `sysmon -accepteula -i`
			- You can add a configuration file later with `sysmon -c c:\windows\config.xml`^[Assuming the config file is in C:\Windows\config.xml]
	- Events are stored in `Applications and Services Logs/Microsoft/Windows/Sysmon/Operational`
- Customizing Sysmon for your environment is a good idea, and as mentioned before, you can add a config file to the install later.
	- [GitHub - SwiftOnSecurity/sysmon-config: Sysmon configuration file template](https://github.com/SwiftOnSecurity/sysmon-config)
	- [GitHub - olafhartong/sysmon-modular: A repository of sysmon configuration modules](https://github.com/olafhartong/sysmon-modular)






# Metadata

### Sources
[Sysmon - Sysinternals | Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
[GitHub - SwiftOnSecurity/sysmon-config: Sysmon configuration file template with default high-quality event tracing](https://github.com/SwiftOnSecurity/sysmon-config)
[GitHub - olafhartong/sysmon-modular: A repository of sysmon configuration modules](https://github.com/olafhartong/sysmon-modular)
[Building A Perfect Sysmon Configuration File | CQURE Academy](https://cqureacademy.com/blog/hacks/sysmon-configuration-file)
### Tags
#tools_win 