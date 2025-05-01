---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/qemu-kv-ms-and-virt-man/installing-a-windows-vm-on-linux/","tags":["VM"],"noteIcon":""}
---

I will eventually create a guide for this, as well as how to connect to a Windows VM with [[Tool Deep-Dives/Linux/Remmina\|Remmina]] over [[Tool Deep-Dives/SSH/SSH\|SSH]] (which is more relevant for a [[Tool Deep-Dives/Proxmox/Proxmox Virtual Environment\|Proxmox]]-hosted VM) or [[Tool Deep-Dives/Linux/Spice\|Spice]]^[Which I discuss in [[Webinars and Training/BlackHills SOC Core/Labs/01-BHIS-SOCC-lab-LinuxHostConfig\|01-BHIS-SOCC-lab-LinuxHostConfig]] if you're impatient.] if you're running it locally, but for now, here's a great series of guides from [sysguides.com](https://sysguides.com) which cover just about everything you need, along with various other guides discussing GPU Passthrough (for gaming, software development, etc.)

1. [How Do I Properly Install KVM on Linux (2024)](https://sysguides.com/install-kvm-on-linux)
	1. It's important to be sure that you follow [section 4](https://sysguides.com/install-kvm-on-linux#3-04-install-virtio-drivers-for-windows-guests-) to install the [[Tool Deep-Dives/Linux/VirtIO\|VirtIO]] drivers on the Linux host machine for Windows guest VMs
2. [How to Properly Install a Windows 11 Virtual Machine on KVM](https://sysguides.com/install-a-windows-11-virtual-machine-on-kvm)
3. Configuring a GPU passthrough
	1. [GitHub - bryansteiner/gpu-passthrough-tutorial](https://github.com/bryansteiner/gpu-passthrough-tutorial)
	2. [GPU virtualization with QEMU/KVM | Ubuntu](https://ubuntu.com/server/docs/gpu-virtualization-with-qemu-kvm)

If you want to improve the power efficiency of the VM and cut bloat/telemetry/etc., you can run run one of several debloat scripts *at the time of Windows 11 installation*. Running these scripts after installation can sometimes cause issues with removing apps, preferences, or dependencies you want to keep.

> **WARNING**: I am not endorsing the use of the scripts below. I advocate making a copy of your VM files before running the scripts to reduce effort in case you need to restore.

[Ultimate Windows Tweaker 5 for Windows 11](https://www.thewindowsclub.com/ultimate-windows-tweaker-5-for-windows-11)

[GitHub - Raphire/Win11Debloat](https://github.com/Raphire/Win11Debloat)

[GitHub - n1snt/Windows-Debloater](https://github.com/n1snt/Windows-Debloater)

