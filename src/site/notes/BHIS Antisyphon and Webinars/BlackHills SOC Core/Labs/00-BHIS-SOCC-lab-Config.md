---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/00-bhis-socc-lab-config/"}
---

First, follow the official guide on downloading and configuring the VM here: [John Strand Training Lab – Download Instructions – Antisyphon Training](https://www.antisyphontraining.com/john-strand-training-lab-download-instructions/)

Unfortunately, the lab VM setup is not always super straight forward. It's a Windows 10 VM with Linux running in Docker, and that presents some problems for most systems. Even if it works out of the box for you and your computer, Windows Updates can make things a little finnicky.

This is my guide/recommendations to configure the VM and have it ready to go.

> **Warm Tip**:^[I always loved this typo in manuals...] quickly disable Windows Update in the VM by modifying local group policy. `gpedit.msc`, expand Computer Configuration>Administrative Templates>Windows Components>Windows Updates.
> Select *Configure Automatic Updates*, click the radio button for *Disable*, then click *Apply*.

# Pre-Config and Setup
## Specs
First, I recommend using a *well-specced machine* with plenty of CPU cores, RAM, and available storage^[Why storage, though? Well, it's faster for your computer to copy/paste a pre-extracted backup of the VM rather than to have 7zip re-extract the VM every time something breaks, and the VM is about 40-60GB when extracted.]. The VM is preconfigured with only *2 cores* and *4GB* RAM, and if you can increase the core count and available memory, the labs will run more quickly.

### Upgrading Specs
Following the instructions provided by BHIS/Antisyphon,^[[John Strand Training Lab – Download Instructions – Antisyphon Training](https://www.antisyphontraining.com/john-strand-training-lab-download-instructions/)] extract the VM, open VMware, and import the VM. Then select the VM, choose "Edit virtual machine settings" in the right column, and increase core count and RAM as you are able.

> If you're limited on processing power, go for N-1 virtual cores, where N is the total physical core/thread count available on your computer.
> - An Intel Core i5 3450 has 4 cores and 4 threads, so you would safely assign 3 cores to the VM.
> - An Intel Core i7 3770K has 4 cores and 8 threads, so you could safely assign 7 cores to the VM.

## RTFM
BHIS provides a VM troubleshooting guide;^[Which is provided in the class, I believe.] if your VM doesn't open when you first try to start it, read it first. 

Depending on the error you get, *you may be tempted to modify the VM*; **don't do it.** The VM uses [Docker](https://www.docker.com/) to run Ubuntu on Windows 10, and your computer needs to be configured properly to handle VMs running VMs. 

Personally, I had to *disable Memory Integrity* on my Windows 11 computers in order for the VM to run properly.
## Disable Updates
Assuming you've now booted into Windows, it's important that you disable System Updates. For that matter, *don't update anything while running the VM unless instructed to do so,* as it might cause problems with how the labs are expected to operate.

Because these VMs are old, Windows Update doesn't "No" for an answer. I'm still working out the best way to disable them, but here are a few things to try.

> **If you just want a script** you can run at machine startup, scroll down, copy it all, paste it, and you should be good for a while.

### Disable Virtual NIC
The most sure-fire way to prevent system updates is to disable the virtual network interface card (NIC) in the VM before booting it up. The problem with doing this, of course, is some labs request network access for package updates, VirusTotal lookups, things like that. But if you don't know the hassle of occasionally powering off to turn the NIC back on, this can save a lot of time and space re-extracting the VM.

Select the VM, click on "Edit virtual machine settings," navigate to "Network" in the left column, and deselect the "Connect at power on."

### Group Policy
Open the start menu, and search for "gpedit"^[you can also do *Windows key+R* and type `gpedit.msc`, but I'm always a little wary of using shortcuts in VMs], and run it. Then drill down from *Computer Configuration>Administrative Templates>Windows Components>Windows Updates*,^[Once you select the first folder, you can quickly drop to the folder by type part of its name, and then use arrow keys to expand it. For example, *A* to select *Administrative Templates*, and *right-arrow* to expand the folder.] and select/open "Configure Automatic Updates" from the list of policies. Disable that shit, you don't need it here!^[You do need it elsewhere though, so unless you've got a WSUS server somewhere managing updates or something like that, don't do this.]

> This does not seem to hold over an extended period of time.

### Disable Services
*Windows Key* + *R*, enter `services.msc`^[you can also search for services in the Start menu and run as admin], scroll down to *Windows Update* (or select the first item and type out "Windows Update" to jump down to it), Right-click>Properties, and change "Startup type" from "Manual" to "Disabled"


# Scripting it
Run Command Prompt as an administrator, then copy and paste the script below into it. In addition to killing the Windows Update and BITS services, it also cleans up your task bar a little, which can be nice when working from laptops or smaller screens.

Note that while it's written for CMD, I have the syntax in PowerShell since most of it is for PowerShell. 

```PowerShell
REM # The first section is written for CMD, which bypasses some PS BS around services.
sc config "wuauserv" start= disabled
sc stop "wuauserv"
sc config "bits" start= disabled
sc stop "bits"
ECHO Switching to powershell!
PowerShell.exe
# Disable Antimalware Realtime Monitoring (required by the Windows CLI Lab)
# This should generate the error "A general error occurred that is not covered by a more specific error code", and means realtime monitoring is already disabled.
Set-MpPreference -DisableRealtimeMonitoring $true
# Cleaning up interface
Write-Host "Beginning modifications, killing Explorer"
TASKKILL /IM explorer.exe /F
# Cortana
Write-Host "Disable Cortana"
Set-ItemProperty -path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" -Name "ShowCortanaButton" -Type DWord -Value 0
# News and Interests
Write-Host "Turning off News and Interests..."
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Feeds" -Name "ShellFeedsTaskbarViewMode" -Type DWord -Value 2
# Windows Search
Write-Host "Remove Windows Search bar (Search still possible in Menu)"
Set-ItemProperty -path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Search" -Name "SearchboxTaskbarMode" -Type DWord -Value 3
# Restart Explorer
Write-Host "Restarting Explorer"
Start-Process explorer.exe
# You're all set
Write-Host "Congratulations! You should be all set with a clean desktop and fine experience for labbing. Hack it!"

```



# Experiments
Create a scheduled task to kill BITS and WUAUSERV.
In its current state, the task is created, but fails to run with 0x2 code.
Unclear if it's necessary since the services are disabled in the main script, but just not taking any risks.
```CMD
REM # Create scheduled task that kills BITS and WUAUSERV at logon.
SCHTASKS /CREATE /SC ONLOGON /TN "MyTasks\Stop BITS" /TR "cmd.exe /c \"net stop bits"" /RL HIGHEST
SCHTASKS /CREATE /SC ONLOGON /TN "MyTasks\Stop WUAUSERV" /TR "cmd.exe /c \"net stop wuauserv"" /RL HIGHEST

```


CMD-only regedits, which were not as clean as PS
```
REM Remove Cortana button
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /t REG_DWORD /reg:32 /v ShowCortanaButton /d 0
Yes

REM Don't show News and Weather
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Feeds /t REG_DWORD /reg:32 /v ShellFeedsTaskbarViewMode /d 2
Yes

REM Don't show News and Weather
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Feeds\FeedRepositorySate /t REG_DWORD /reg:32 /v FeedEnabled /d 0
Yes

REM Get rid of the Windows search bar
REM Remember that you can still search whenever/however you want by opening the start menu. Same deal.
reg add HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Search /t REG_DWORD /reg:32 /v SearchboxTaskbarMode /d 3
Yes

```