---
{"dg-publish":true,"permalink":"/tool-deep-dives/proxmox/proxmox-pve-admin-guide/","updated":"2023-12-12T12:56:43.000-08:00"}
---


[Proxmox VE Administration Guide](https://pve.proxmox.com/pve-docs/pve-admin-guide.html)

There is a *Post Install* script that can be found [here by tteck](https://tteck.github.io/Proxmox/) which goes through changing the repos, disabling unnecessary services, and stopping the subscription nag. The person also has a bunch of other scripts there which can do a lot, so check it out!
## Important things to note:
1. Don't use a `.local` [[Tool Deep-Dives/Proxmox/Changing Proxmox Server Name and Domain\|domain name]].
2. Change the subscription repository
	1. If you're getting `TASK ERROR: command 'apt-get update' failed: exit code 100` in your update output, you need to change your repos from *Enterprise* to *No Subscription*
		1. Navigate to your Host, then scroll down to Updates, then Repositories, and select any repo that has "*enterprise*" in the *Components* column and *disable* it.
		2. Click on *Add*, and add the *No-Subscription* versions you just disabled
		3. ![Proxmox PVE admin guide-1.png](/img/user/Attachments/Proxmox%20PVE%20admin%20guide-1.png)
		4. Hit *Reload*, then click on *Updates* in the main node menu, and click *Refresh* to check for updates. It should resolve to *Task OK*, and you can install updates by clicking *Upgrade!*
		5. If you installed a kernel update, you probably need to reboot.
3. You should probably try to use the *GUI as much as possible* to avoid accidentally screwing yourself by accident.
	1. Like sure, you could [[Tool Deep-Dives/Proxmox/Adding drives to Proxmox\|add drives to Proxmox]] using the CLI, but then they don't show up as added storage because you didn't enter the command to activate it or something and you can't be bothered to look up how to do it.
4. Don't run `apt upgrade`, use `apt distro-upgrade`, or you [**will have problems**](https://www.reddit.com/r/Proxmox/comments/ujqig9/use_apt_distupgrade_or_the_gui_not_apt_upgrade/).
	1. If you're totally new to Proxmox, it's probably easier to just reinstall and go forward.
	2. If you're in a bind, however, you can try to uninstall the upgrades you did - I did not test these, I just reinstalled because why start off on the wrong foot?
		1. [debian - Can I rollback an apt-get upgrade if something goes wrong? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/79050/can-i-rollback-an-apt-get-upgrade-if-something-goes-wrong)
		2. [How To rollback an apt-get/apt upgrade on Debian/Ubuntu Linux - nixCraft](https://www.cyberciti.biz/howto/debian-linux/ubuntu-linux-rollback-an-apt-get-upgrade/)

