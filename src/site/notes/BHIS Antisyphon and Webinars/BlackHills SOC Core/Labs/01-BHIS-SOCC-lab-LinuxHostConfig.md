---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/01-bhis-socc-lab-linux-host-config/"}
---

> Download John Strand's VM from here: [John Strand Training Lab – Download Instructions – Antisyphon Training](https://www.antisyphontraining.com/john-strand-training-lab-download-instructions/)

# Running VM on Linux
This is a quick and dirty guide to get the VM working on a Linux system; I got this running on Kubuntu 24.04, so you may have different required dependencies.

You might be tempted to use VirtMan, but I wasn't able to get it to work. This is because VirtMan doesn't support virtualized NVMe drives, even though QEMU does. There's quite a lot of discussion about it at this blog post ([KVM guests with emulated SSD and NVMe drives – Just another Linux geek](https://blog.christophersmart.com/2019/12/18/kvm-guests-with-emulated-ssd-and-nvme-drives/)), and maybe I'll tackle that later, but right now I wanted to make sure it could be done first.

With that out of the way, let's get started! 
## Install dependencies
These are broken up so you can more easily see what's being installed and why.
```shell
## Update repos
sudo apt-get update

## Install 7zip
sudo apt install 7zip

## Install QEMU and KVM
sudo apt-get install qemu-system-x86

## Install OVMF (UEFI firmware)
sudo apt-get install ovmf

## Install SPICE server and client tools
sudo apt-get install spice-client-gtk virt-viewer

## Install Virtio drivers
sudo apt-get install qemu-utils

## Optionally, install libvirt and virt-manager for easier management
### Haven't yet actually gotten virtman to work this, so...
sudo apt-get install libvirt-daemon-system libvirt-clients virt-manager
```

### tl;dr dependencies
Here's everything condensed to save you some time. Run just the first section to install the basics, and the second section install for VirtManager.

```Shell
# Install basic utils
sudo apt update && sudo apt install 7zip qemu-system-x86 ovmf spice-client-gtk virt-viewer qemu-utils -Y

# Install VirtManager
sudo apt install libvirt-daemon-system libvirt-clients virt-manager -Y
```

## Converting and Using the VM
To get started, you should extract the VM into a location you want to use.

```shell
# Create the folder VirtMachs in Documents and download the virtual machine to it.
mkdir ~/Documents/VirtMachs && wget -O ~/Documents/VirtMachs/WINADHD04_23.7z https://introclassjs.s3.us-east-1.amazonaws.com/WINADHD04_23.7z

# CD to the folder and extract the downloaded file to a folder sharing its name.
cd ~/Documents/VirtMachs && 7z x WINADHD04_23.7z

# Convert the VM with a progress bar
cd ~/Documents/VirtMachs/WINADHD04_23 && qemu-img convert -p -O qcow2 WINADHD-disk1.vmdk WINADHD-disk1.qcow2

# Start the VM from the Terminal (choose one)

## Potato system (default config, 2 cores/4GB RAM)
cd ~/Documents/VirtMachs/WINADHD04_23 && qemu-system-x86_64 -drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1 -device nvme,drive=disk1,serial=deadbeef -m 4096 -smp cores=2 -enable-kvm -cpu host -bios /usr/share/ovmf/OVMF.fd -vga virtio -spice port=5930,disable-ticketing=on -device virtio-serial-pci -chardev spicevmc,id=vdagent,debug=0,name=vdagent -device virtserialport,chardev=vdagent,name=com.redhat.spice.0

## Average system (6 cores/6GB RAM)
cd ~/Documents/VirtMachs/WINADHD04_23 && qemu-system-x86_64 -drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1 -device nvme,drive=disk1,serial=deadbeef -m 6144 -smp cores=4 -enable-kvm -cpu host -bios /usr/share/ovmf/OVMF.fd -vga virtio -spice port=5930,disable-ticketing=on -device virtio-serial-pci -chardev spicevmc,id=vdagent,debug=0,name=vdagent -device virtserialport,chardev=vdagent,name=com.redhat.spice.0

## Beefy system (8 cores/10GB RAM)
cd ~/Documents/VirtMachs/WINADHD04_23 && qemu-system-x86_64 -drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1 -device nvme,drive=disk1,serial=deadbeef -m 10240 -smp cores=8 -enable-kvm -cpu host -bios /usr/share/ovmf/OVMF.fd -vga virtio -spice port=5930,disable-ticketing=on -device virtio-serial-pci -chardev spicevmc,id=vdagent,debug=0,name=vdagent -device virtserialport,chardev=vdagent,name=com.redhat.spice.0
```

If all went well, then the VM is now running; however, you won't be able to see anything but a blinking cursor.

Now you need top open a separate terminal and launch SPICY to view the VM.
```shell
# View the VM with Spicy
spicy --port 5930
```

If something goes wrong while you're playing around or setting up the VM,^[I had lots of weird device issues getting this figured out...] you can always just convert the original VMDK to qcow2, overwriting the bad qcow2 VM, and try again.^[Also, it turns out you don't have to convert the VMDK to qcow2 for QEMU to read it; just change the `-drive file=WINADHD-disk1.qcow2,format=qcow2` to `-drive file=WINADHD-disk1.vmdk,format=vmdk` and it should work.]

### Running Without SPICE
If you just want to run everything from one terminal and don't care about copy/paste, you can use the following code to just run it; it will pop-up while booting, so you'll be able to see any BIOS errors or problems immediately.

```shell
# Potato system (default config, 2 cores/4GB RAM)
qemu-system-x86_64 -drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1 -device nvme,drive=disk1,serial=deadbeef -m 4096 -smp cores=2 -enable-kvm -cpu host -bios /usr/share/ovmf/OVMF.fd -vga virtio

# Average system (6 cores/6GB RAM)
qemu-system-x86_64 -drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1 -device nvme,drive=disk1,serial=deadbeef -m 6144 -smp cores=6 -enable-kvm -cpu host -bios /usr/share/ovmf/OVMF.fd -vga virtio

# Beefy system (8 cores/10GB RAM)
qemu-system-x86_64 -drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1 -device nvme,drive=disk1,serial=deadbeef -m 10240 -smp cores=8 -enable-kvm -cpu host -bios /usr/share/ovmf/OVMF.fd -vga virtio
```

This basically just leaves off the last few switches that manage SPICE.
## CRITICAL: After-boot Setup
Once you're booted into Windows, there are two things you will want to do immediately.

1. Stop updates
	1. Critical for the labs to work
	2. *Must be completed as soon as possible after booting*
2. Install SPICE and VirtIO guest tools
	1. Helpful to manage the vm, like being able to copy/paste between the host and vm
	2. We'll also download and install the SPICE guest tools, and lastly the VirtIO drivers.^[which will allow you to dynamically resize the window.]

Because you can't copy/paste into the system yet, I've  added ShortURL links to make typing them in a little faster.

### Quickest Method
The quickest way to stop Windows Updates and install the latest SPICE Guest Tools is to go to the ShortURL link below and copy/paste the entire script into an Elevated PowerShell session.

[https://shorturl.at/0xPiV](https://shorturl.at/0xPiV)

*You will need click through the install wizards and hit Enter in the terminal when prompted.*

Below in the manual steps is an explanation of what's going on.
### Manual Steps
1. Stopping updates
	1. Open edge quickly and manually go to https://github.com/WiseGuru/Windows-Lab-VM-Update-Stomp
		1. Shortened URL: https://shorturl.at/NwUHK
			1. Capitalization is important.
		2. Open "KillWindowsUpdates.ps1" and select the copy icon from the top right
			1. If you use the shortened URL, it takes you right to the script.
	2. Run Powershell as admin and paste the command into it
		1. Once the terminal is open, you can just right-click and the script will paste and run itself
		2. It's explained in detail in [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/00-BHIS-SOCC-lab-Config\|00-BHIS-SOCC-lab-Config]]
2. Install SPICE guest tools
	1. Open Edge and navigate to https://www.spice-space.org/download/windows/spice-guest-tools/spice-guest-tools-latest.exe
		1. Shortened URL: https://shorturl.at/bQ7eR
			1. NOTE: This is a direct link to the EXE, so you will first be taken to the "Short URL" page which is full of ads, but the file should download.
	2. Run the executable to install the drivers.
3. Install the VirtIO drivers and then reboot.
	1. Use [this link to the latest drivers](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/) and either download the MSI you're looking for or the `virtio-win-guest-tools.exe`
		1. You should be able to copy/paste the link above, bit if not, here's the shortened URL: https://shorturl.at/7xOg6
	2. Run the installer, and at the end you will be prompted to reboot; do that now.
4. You should be set!

> A quick note on screen resolution; if you want to use a *standard* screen resolution (720p, 1080p, etc.), you can change the settings as normal in Windows. However, if you want it to take on the resolution of the SPICE window as you've sized it, rebooting the VM is the quickest method.

## Explaining the launch command
So that launch command is a bit long, so let's dive into it and explain what it's doing.

 >If/when you're doing this for another VM, you can find the original configuration in the `virtualmachine.vmx` file; that's how we know this machine was originally configured to use NVMe drives.

1. `qemu-system-x86_64`
	1. The command to run the emulator.
2. `-drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1`
	1. Selects the drive we created earlier, and assigns it the ID disk1.
	2. `if=none` doesn't assign it to an interface.
	3. *Alternate*: `-drive file=WINADHD-disk1.vmdk,format=vmdk,if=none,id=disk1`
		1. Uses the original VMDK to launch the VM. Should work just fine.
3. `-device nvme,drive=disk1,serial=deadbeef`
	1. Setups up the drive to be NVME with serial [Deadbeef](https://en.wikipedia.org/wiki/Deadbeef).
4. `-m 4096 -smp cores=2`
	1. Configures it with 4GB RAM and 2 CPU cores.
	2. Feel free to change these as you like.
5. `-enable-kvm -cpu host`
	1. Enables KVM hardware acceleration and passes through the CPU configuration.
	2. This DRAMATICALLY improves performance on the virtual machine.
6. `-bios /usr/share/ovmf/OVMF.fd`
	1. Sets the BIOS to UEFI.
7. `-vga virtio`
	1. Instructs the VM to use VirtIO display drivers, which allows us to resize the display automatically.
8. `-spice port=5930,disable-ticketing=on`
	1. Sets the SPICE port number and disables authentication to access.
	2. Note that `disable-ticketing` in short has been deprecated, so `disable-ticketing=on` is the appropriate switch.
9. `-device virtio-serial-pci`
	1. Required serial PCI device for SPICE.
10. `-chardev spicevmc,id=vdagent,debug=0,name=vdagent`
	1. *Character device definition* with an ID of `vdagent`
	2. Sets up a communication channel^[SPICE Virtual Machine Channel (VMC)] between the SPICE client and the guest VM.
	3. The character device acts as an endpoint that can be used for various SPICE-related functionalities, such as clipboard sharing and file transfers.
11. `-device virtserialport,chardev=vdagent,name=com.redhat.spice.0`
	1. Creates a Virtio serial port device within the guest VM.
	2. This serial port is linked to the SPICE VMC character device created earlier (`vdagent`). 
	3. The name `com.redhat.spice.0` is recognized by the SPICE agent running inside the guest VM as the channel for SPICE communication.

# Sources
- https://www.kevindiaz.dev/blog/running-vmware-images-in-qemu.html
	- Initial blog post inspiring the conversion.
- [qemu-system-x86\_64(1) Debian Manpages](https://manpages.debian.org/testing/qemu-system-x86/qemu-system-x86_64.1.en.html)
	- Additional context and information for launching the VM.
- [FAQ - SPICE](https://www.spice-space.org/faq.html)
	- Helpful for troubleshooting and understanding functionality.


# Changelog
- 08/25/2024 2:25 PT
	- Created a new script to configure the VM as quickly as possible
	- Added VM configurations with variable specs.
	- General reformatting
- 06/24/2024 1:25 PT
	- Added VirtIO display drivers/instructions./
- 06/20/2024 9:00 PT
	- General cleanup and explanation
- 06/20/2024 5:12 PT
	- Initial publication