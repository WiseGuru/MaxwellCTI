---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/qemu-kv-ms-and-virt-man/adding-a-second-monitor-to-a-qemu-vm/","tags":["VM"],"noteIcon":""}
---

> In progress and possibly untested; take everything with a pinch of salt.


[command line - Adding a second monitor on qemu virtual machine - Super User](https://superuser.com/questions/1655709/adding-a-second-monitor-on-qemu-virtual-machine)


[First Answer](https://superuser.com/posts/1760266/timeline)

I recently tried to set up a virtual machine with several video heads (e.g., two monitors).

I found a series of blog articles that explain how to do that: [https://linux-blog.anracom.com/2017/07/06/kvmqemu-mit-qxl-hohe-aufloesungen-und-virtuelle-monitore-im-gastsystem-definieren-und-nutzen-i/](https://linux-blog.anracom.com/2017/07/06/kvmqemu-mit-qxl-hohe-aufloesungen-und-virtuelle-monitore-im-gastsystem-definieren-und-nutzen-i/)

Unfortunately, it is written in German, a language I can’t read. But an online translater helped me, and I finally succeeded running a virtual machine with two heads.

As I understand it, you are missing two things:

1. You need to configure the `qxl-vga` device to enable two heads with the `max_outputs=2` option (you can use values up to 4).
2. You have to use a spice client that supports multiple displays over a single connection. (On my Debian host, `remote-viewer`, from the `virt-viewer` package does, but `spicy` does not.)

For the reference, here is the command I used to start an Ubuntu guest, with dual-head support:

```
qemu-system-x86_64 -machine q35,accel=kvm -cpu host -smp 1 -m size=4G \
-audiodev driver=spice,id=audio -device intel-hda -device hda-duplex,audiodev=audio \
-blockdev driver=raw,node-name=ubuntu,file.driver=file,file.filename=Downloads/ubuntu-22.04.1-desktop-amd64.iso \
-device ide-cd,drive=ubuntu \
-vga none -device qxl-vga,vgamem_mb=64,ram_size_mb=256,vram_size_mb=128,max_outputs=2 \
-device piix4-usb-uhci -device usb-tablet \
-display none -spice port=5900,addr=127.0.0.1,disable-ticketing \
-chardev spicevmc,id=charchannel0,name=vdagent \
-device virtio-serial-pci,id=virtio-serial0 -device virtserialport,bus=virtio-serial0
```


