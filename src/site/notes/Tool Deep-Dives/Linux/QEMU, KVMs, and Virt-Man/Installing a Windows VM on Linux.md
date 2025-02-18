---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/qemu-kv-ms-and-virt-man/installing-a-windows-vm-on-linux/","tags":["VM"]}
---

I will eventually create a guide for this, as well as how to connect to a Windows VM with [[Tool Deep-Dives/Linux/Remmina\|Remmina]] over [[Tool Deep-Dives/SSH/SSH\|SSH]] (which is more relevant for a [[Tool Deep-Dives/Proxmox/Proxmox Virtual Environment\|Proxmox]]-hosted VM) or [[Tool Deep-Dives/Linux/Spice\|Spice]]^[Which I discuss in [[Webinars and Training/BlackHills SOC Core/Labs/01-BHIS-SOCC-lab-LinuxHostConfig\|01-BHIS-SOCC-lab-LinuxHostConfig]] if you're impatient.] if you're running it locally, but for now, here's a great series of guides from [sysguides.com](https://sysguides.com) which covers just about everything you need.

1. [How Do I Properly Install KVM on Linux (2024)](https://sysguides.com/install-kvm-on-linux)
	1. It's important to be sure that you follow [section 4](https://sysguides.com/install-kvm-on-linux#3-04-install-virtio-drivers-for-windows-guests-) to install the [[Tool Deep-Dives/Linux/VirtIO\|VirtIO]] drivers on the Linux host machine for Windows guest VMs
2. [How to Properly Install a Windows 11 Virtual Machine on KVM](https://sysguides.com/install-a-windows-11-virtual-machine-on-kvm)


