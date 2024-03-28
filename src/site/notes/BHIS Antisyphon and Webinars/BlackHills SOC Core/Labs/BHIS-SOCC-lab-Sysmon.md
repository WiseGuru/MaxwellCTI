---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/bhis-socc-lab-sysmon/"}
---

## LAB: Sysmon Log
[IntroLabs/IntroClassFiles/Tools/IntroClass/Sysmon/Sysmon.md at master · strandjs/IntroLabs · GitHub](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/Sysmon/Sysmon.md)

In this lab, we're looking at Sysmon and the kinds of logs it returns.

The original lab has you create/run a piece of malware, then install Sysmon, and see what kinds of logs you see. This is neat, but I want to see a kind of before and after.

So the first thing we need to do is uninstall Sysmon and remove it from the VM; maybe this will cause problems with future labs, but you can always recreate the VM from scratch.

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

Note that the output is written to a file in the 


[GitHub - abhinav-eyesOnglass/evtx: Merge evtx files](https://github.com/abhinav-eyesOnglass/evtx)



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

```


### Lab work
1. Create some malware
2. Begin Sysmon (should already be running)
3. Investigate Sysmon Logs (Application-Microsoft-Windows-Sysmon/Operational)
	1. Once you find the event, you can find a TON of information about that event
4. [GPO and Sysmon](https://www.syspanda.com/index.php/2017/02/28/deploying-sysmonthrough-gpo/)