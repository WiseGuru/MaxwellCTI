---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/01-bhis-socc-lab-linux-host-config/"}
---

# Converting The VM to run on Linux
This is a quick and dirty guide to get the VM working on a Linux system; I got this running on Kubuntu 24.04, so you may have different required dependencies.

You might be tempted to use VirtMan, but I wasn't able to get it to work. There's quite a lot of discussion about it at this blog post ([KVM guests with emulated SSD and NVMe drives – Just another Linux geek](https://blog.christophersmart.com/2019/12/18/kvm-guests-with-emulated-ssd-and-nvme-drives/)), and maybe I'll tackle that later, but right now I wanted to make sure it could be done first.

So this will  get it done!
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
Here's everything in two lines to save you some time. Copy the first line to ignore VirtManager, copy both to do everything.

```Shell
sudo apt update && sudo apt install 7zip qemu-system-x86 ovmf spice-client-gtk virt-viewer qemu-utils -Y
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
qemu-img convert -p -O qcow2 WINADHD-disk1.vmdk WINADHD-disk1.qcow2

# Start the VM from the Terminal
qemu-system-x86_64 -drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1 -device nvme,drive=disk1,serial=deadbeef -m 4096 -smp cores=4 -enable-kvm -cpu host -bios /usr/share/ovmf/OVMF.fd -spice port=5930,disable-ticketing=on -device virtio-serial-pci -chardev spicevmc,id=vdagent,debug=0,name=vdagent -device virtserialport,chardev=vdagent,name=com.redhat.spice.0

# View the VM with Spicy
spicy --port 5930
```

If something goes wrong while you're playing around or setting up the VM, you can always just convert the original VMDK to qcow2, overwriting the bad qcow2 VM, and try again.

## CRITICAL: After-boot Setup
So you've just gone through all this trouble to download and convert this VM, so the next steps are critical to ensure updates don't get installed on Windows.

1. Once you're booted into Windows, there are two things you will want to do immediately.
	1. Stop updates
		1. Critical for the labs to work
	2. Install Spice guest tools
		1. Helpful to manage the vm, like being able to copy/paste between the host and vm
2. Stopping updates
	1. Open edge quickly and manually go to https://github.com/WiseGuru/Windows-Lab-VM-Update-Stomp
		1. Open "KillWindowsUpdates.ps1" and select the copy icon from the top right
	2. Run Powershell as admin and paste the command into it
		1. It's explained in detail in [[BHIS Antisyphon and Webinars/BlackHills SOC Core/Labs/00-BHIS-SOCC-lab-Config\|00-BHIS-SOCC-lab-Config]]
3. Install Spice guest tools
	1. Open Edge and navigate to https://docs.getutm.app/guest-support/windows/#download
		1. Download the ISO
	2. Open/Mount the ISO and run the setup EXE
	3. Once complete, reboot the VM and you should be set!

## Explaining the launch command
So that launch command is a bit of a mouthful, so let's dive into it and explain what it's doing.

1. `qemu-system-x86_64`
	1. The command to run the emulator
2. `-drive file=WINADHD-disk1.qcow2,format=qcow2,if=none,id=disk1`
	1. Selects the drive we created earlier, and assigns it the ID disk1
	2. `if=none` doesn't assign it to an interface
3. `-device nvme,drive=disk1,serial=deadbeef`
	1. Setups up the drive to be NVME with serial [Deadbeef](https://en.wikipedia.org/wiki/Deadbeef)
4. `-m 4096 -smp cores=4`
	1. Configures it with 4GB RAM and 4 CPU cores
	2. Feel free to change these as you like
5. `-enable-kvm -cpu host`
	1. Enables KVM hardware acceleration and passes through the CPU configuration
	2. This DRAMATICALLY improves performance on the virtual machine
6. `-bios /usr/share/ovmf/OVMF.fd`
	1. Sets the BIOS to UEFI
7. `-spice port=5930,disable-ticketing=on`
	1. Sets the SPICE port number and disables authentication to access
8. `-device virtio-serial-pci`
	1. Required serial PCI device for SPICE
9. `-chardev spicevmc,id=vdagent,debug=0,name=vdagent`
	1. More spice stuff
10. `-device virtserialport,chardev=vdagent,name=com.redhat.spice.0`
	1. Even more spice stuff