---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/00-bhis-socc-lab-config/"}
---

> Download John Strand's VM from here: [John Strand Training Lab – Download Instructions – Antisyphon Training](https://www.antisyphontraining.com/john-strand-training-lab-download-instructions/)

# Kill Windows Update
If you just want to download the script to kill Windows Updates, go here:

## [GitHub: KillWindowsUpdates.ps1](https://github.com/WiseGuru/Windows-Lab-VM-Update-Stomp/blob/main/KillWindowsUpdates.ps1)

To run it on your VM, open the link above, click the "Copy" icon at the top right (next to Raw, two squares, etc.), start an Elevated PowerShell session on the VM, and paste the script into the terminal.

This script will:
1. *Kill* any active update process
2. *Disable* all update services
3. *Create a task* that kills processes and disables update services to run at login and then every 15 minutes (you are welcome to adjust this lower, but I didn't find it necessary)
4. *Disable Antimalware Realtime Monitoring* (which is required by at least one lab)
5. *Clean up the interface* (disable Cortana, hide News and Interests, hide the Search bar because you can just open the start menu and type to search, and show full file extensions.) and *restart Explorer* to make the changes take effect.

# Pre-Config and Setup

If you're running this VM on a Windows system, configuration can be a little tricky. It's a Windows 10 VM with Linux running in Docker, and that presents some problems for most systems. Even if it works out of the box for you and your computer, Windows Updates can break your VM and stop labs from working correctly.

This is my guide/recommendations to configure the VM and have it ready to go.
## Specs
First, I recommend using a *well-specced machine* with plenty of CPU cores, RAM, and available storage^[Why storage, though? Well, it's faster for your computer to copy/paste a pre-extracted backup of the VM rather than to have 7zip re-extract the VM every time something breaks, and the VM is about 40-60GB when extracted.]. The VM is preconfigured with only *2 cores* and *4GB* RAM, and if you can increase the core count and available memory, the labs will run more quickly.

### Upgrading VM Specs
Following the instructions provided by BHIS/Antisyphon,^[[John Strand Training Lab – Download Instructions – Antisyphon Training](https://www.antisyphontraining.com/john-strand-training-lab-download-instructions/)] extract the VM, open VMware, and import the VM. Then select the VM, choose "Edit virtual machine settings" in the right column, and increase core count and RAM as you are able.

> If you're limited on processing power, go for N-1 virtual cores, where N is the total physical core/thread count available on your computer.
> - An Intel Core i5 3450 has 4 cores and 4 threads, so you would safely assign 3 cores to the VM.
> - An Intel Core i7 3770K has 4 cores and 8 threads, so you could safely assign 7 cores to the VM.

## RTFM
BHIS provides a VM troubleshooting guide;^[Which is provided in the class, I believe.] if your VM doesn't open when you first try to start it, read it first. 

If you get a weird error, check the troubleshooting guide, and ask for help in [the Discord.](https://discord.gg/antisyphon)

When in doubt, change the *host* system settings, and *not the VM*; Personally, I had to *[[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/Disable Windows Memory Integrity\|Disable Windows Memory Integrity]]* on my Windows 11 computers in order for the VM to run properly.
## Troubleshooting: Disable Updates
Assuming you've now booted into Windows, it's important that you disable System Updates. For that matter, *don't update anything while running the VM unless instructed to do so,* as it might cause problems with how the labs are expected to operate.

> You will know if Windows Update kicks off, because you will no longer by able to open the "Terminal" shortcuts on the Desktop or Taskbar as Admin (they just won't open).
> If that happens, you're running on borrowed time, and it might be worth doing a fresh extraction of the VM.

### [This is my GitHub repo with my script to stop Windows Updates.](https://github.com/WiseGuru/Windows-Lab-VM-Update-Stomp) 
It's worked well for me so far, but if you have trouble, below are some of the steps I've tried to disable updates.

### Disable Virtual NIC
The most sure-fire way to prevent system updates is to disable the virtual network interface card (NIC) in the VM before booting it up. The problem with doing this, of course, is some labs request network access for package updates, VirusTotal lookups, things like that. But if you don't know the hassle of occasionally powering off to turn the NIC back on, this can save a lot of time and space re-extracting the VM.

Select the VM, click on "Edit virtual machine settings," navigate to "Network" in the left column, and deselect the "Connect at power on."

### Group Policy
> **NOTE**: This does not seem to hold over an extended period of time.

Open the start menu, and search for "gpedit"^[you can also do *Windows key+R* and type `gpedit.msc`, but I'm always a little wary of using shortcuts in VMs], and run it. Then drill down from *Computer Configuration>Administrative Templates>Windows Components>Windows Updates*,^[Once you select the first folder, you can quickly drop to the folder by type part of its name, and then use arrow keys to expand it. For example, *A* to select *Administrative Templates*, and *right-arrow* to expand the folder.] and select/open "Configure Automatic Updates" from the list of policies. Disable that shit, you don't need it here!^[You do need it elsewhere though, so unless you've got a WSUS server somewhere managing updates or something like that, don't do this.]

### Disable Services
*Windows Key* + *R*, enter `services.msc`^[you can also search for services in the Start menu and run as admin], scroll down to *Windows Update* (or select the first item and type out "Windows Update" to jump down to it), Right-click>Properties, and change "Startup type" from "Manual" to "Disabled".

There's also the *BITS* service, which is the background file-transfer service Windows uses to manage update files, and the *Windows Update Orchestrator Service,* which manages all things Windows Update.^[[How Windows Update works - Windows Deployment | Microsoft Learn](https://learn.microsoft.com/en-us/windows/deployment/update/how-windows-update-works)]

# Scripting it
Run PowerShell as an administrator, then copy and paste the script below into it. In addition to killing the Windows Update and BITS services, it also cleans up your task bar a little and enables seeing all file extensions. Thoroughly commented so you can modify it as you like.

I have moved the script! I realized that the best way to openly and collaboratively troubleshoot the script was to publish it on GitHub. The link to the script is here:

## [GitHub: KillWindowsUpdates.ps1](https://github.com/WiseGuru/Windows-Lab-VM-Update-Stomp/blob/main/KillWindowsUpdates.ps1)

And for those who get lost in GitHub^[Definitely not me ever.], here's a link to the changes I've made.

[Commits · WiseGuru/Windows-Lab-VM-Update-Stomp](https://github.com/WiseGuru/Windows-Lab-VM-Update-Stomp/commits/main/)

To verify that these services have not started back up again, you can run the following command to get their status:

```PowerShell
Get-Service -Name wuauserv,bits,UsoSvc,InstallService
```
The output should look like this:
![00-BHIS-SOCC-lab-Config.png](/img/user/Attachments/00-BHIS-SOCC-lab-Config.png)


# Metadata

### Sources
