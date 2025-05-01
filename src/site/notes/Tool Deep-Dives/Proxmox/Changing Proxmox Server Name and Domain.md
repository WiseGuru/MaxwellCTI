---
{"dg-publish":true,"permalink":"/tool-deep-dives/proxmox/changing-proxmox-server-name-and-domain/","noteIcon":""}
---



[Renaming a PVE node - Proxmox VE](https://pve.proxmox.com/wiki/Renaming_a_PVE_node)
[[SOLVED] - Promox cluster domain changing | Proxmox Support Forum](https://forum.proxmox.com/threads/promox-cluster-domain-changing.111578/)

I made this mistake using a `.local` [domain](https://community.veeam.com/blogs-and-podcasts-57/why-using-local-as-your-domain-name-extension-is-a-bad-idea-4828), and so I think the above forum/guide discusses how to change this.

I didn't see a domain name in the `/etc/hostname` file, just in `/etc/hosts`