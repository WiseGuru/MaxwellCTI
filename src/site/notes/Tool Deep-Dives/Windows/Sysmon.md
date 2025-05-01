---
{"dg-publish":true,"permalink":"/tool-deep-dives/windows/sysmon/","noteIcon":""}
---

#### Sysmon
- _System Monitor_ (_Sysmon_) is a Windows system service and device driver that, once installed on a system, remains resident across system reboots to monitor and log system activity to the Windows event log.^[[Sysmon - Sysinternals | Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)]
	- There is also a [Sysmon for Linux](https://github.com/Sysinternals/SysmonForLinux) to allow logging
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


[A Sysmon Event ID Breakdown - Updated to Include 29!! - Black Hills Information Security](https://www.blackhillsinfosec.com/a-sysmon-event-id-breakdown/)
[Deploying Sysmon through Group Policy (GPO) \*Updated scroll down\* - Syspanda](https://www.syspanda.com/index.php/2017/02/28/deploying-sysmon-through-gpo/)
[GitHub - nsacyber/Event-Forwarding-Guidance: Configuration guidance for implementing collection of security relevant Windows Event Log events by using Windows Event Forwarding. #nsacyber](https://github.com/nsacyber/Event-Forwarding-Guidance)

WEC Server
[Windows Event Collector - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/wec/windows-event-collector)
# Metadata

### Sources
[Sysmon - Sysinternals | Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
[GitHub - SwiftOnSecurity/sysmon-config: Sysmon configuration file template with default high-quality event tracing](https://github.com/SwiftOnSecurity/sysmon-config)
[GitHub - olafhartong/sysmon-modular: A repository of sysmon configuration modules](https://github.com/olafhartong/sysmon-modular)
[Building A Perfect Sysmon Configuration File | CQURE Academy](https://cqureacademy.com/blog/hacks/sysmon-configuration-file)
[Sysmon Threat Analysis Guide](https://www.varonis.com/blog/sysmon-threat-detection-guide)
### Tags
#tools_win 