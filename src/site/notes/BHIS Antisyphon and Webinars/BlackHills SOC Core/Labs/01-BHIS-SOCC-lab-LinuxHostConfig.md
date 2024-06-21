---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/01-bhis-socc-lab-linux-host-config/"}
---


This is a quick and dirty guide to get the VM working on a Linux system; I got this running on Kubuntu 24.04, so you may have different required dependencies.

```shell
# Install dependencies

## Update repos
sudo apt-get update

## Install QEMU and KVM
sudo apt-get install qemu-kvm qemu-system-x86

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

Once everything is installed, you can move on to configuring and running the VM.

```shell
# Convert and run the VM

# Extract, then convert the VM
qemu-img convert -p -O qcow2 WINADHD-disk1.vmdk WINADHD-disk1.qcow2

# Start the VM from the Terminal
qemu-system-x86_64 -drive file=WINADHD-disk2.qcow2,format=qcow2,if=none,id=disk1 -device nvme,drive=disk1,serial=deadbeef -m 4096 -smp cores=8 -enable-kvm -cpu host -bios /usr/share/ovmf/OVMF.fd -spice port=5930,disable-ticketing=on -device virtio-serial-pci -chardev spicevmc,id=vdagent,debug=0,name=vdagent -device virtserialport,chardev=vdagent,name=com.redhat.spice.0

# View the VM with Spicy
spicy --port 5930
```

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