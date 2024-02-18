---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/00-bhis-socc-lab-config/"}
---

Unfortunately, the lab VM setup is not super straight forward. Even if it works out of the box for you and your computer, it will get a little finicky.

This is my guide/recommendations to configure the VM and have it ready to go.

> **Warm Tip**:^[I always loved this typo in manuals...] quickly disable Windows Update in the VM by modifying local group policy. `gpedit.msc`, expand Computer Configuration>Administrative Templates>Windows Components>Windows Updates.
> Select *Configure Automatic Updates*, click the radio button for *Disable*, then click *Apply*.

# Pre-Config and Setup
## Specs
First, I recommend using a *well-specced machine* with plenty of cores, RAM, and available storage. The VM is preconfigured with only *2 cores* and *4GB* RAM, and if you can increase the core count and available memory, the labs will run more quickly.

Why storage, though? Well, it's faster for your computer to copy/paste a pre-extracted backup of the VM rather than to have 7zip re-extract the VM every time something breaks, and the VM is about 40-60GB when extracted. 
### Upgrading Specs
Extract the VM, open VMware and point it to the VM, and increase core count and RAM as you are able.

## RTFM
BHIS provides a VM troubleshooting guide; if your VM doesn't open when you first try to start it, read it first. 

Depending on the error you get, *you may be tempted to modify the VM*; **don't do it.** The VM uses [Docker](https://www.docker.com/) to run Ubuntu on Windows 10, and your computer needs to be configured properly to handle VMs running VMs. 

I had to *disable Memory Integrity* on my computer in order for the VM to run properly.
## Disable Updates
Assuming you've now booted into Windows, it's important that you disable System Updates. *For that matter, don't update anything while running the VM, as it might cause problems with how the labs are expected to operate.*

Because these VMs are old, and they've had "Do Not Update" so often that it doesn't take "No" for an answer. IMO, the best way to prevent updates from breaking your build is to disable them in the local group policy.

Open the start menu, and search for "gpedit"^[you can also do *Windows key+R* and type `gpedit.msc`, but I'm always a little wary of using shortcuts in VMs], and run it. Then drill down from *Computer Configuration>Administrative Templates>Windows Components>Windows Updates*,^[Once you select the first folder, you can quickly drop to the folder by type part of its name, and then use arrow keys to expand it. For example, *A* to select *Administrative Templates*, and *right-arrow* to expand the folder.] and select/open "Configure Automatic Updates" from the list of policies. Disable that shit, you don't need it here!^[You do need it elsewhere though, so unless you've got a WSUS server somewhere managing updates or something like that, don't do this.] 
