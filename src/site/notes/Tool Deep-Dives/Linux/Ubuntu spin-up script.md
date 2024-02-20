---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/ubuntu-spin-up-script/"}
---


Completely untested, really posting for my own use
### Host Script
#### Objectives
1. Install Resilio Sync
	1. Modify service to set as username
2. Install Obsidian
3. Install Flameshot
4. Install 1Password
5. Install KVM and Virt-Manager
6. Install DisplayLink drivers
7. 1Password
8. TIDAL

##### Need to add
1. Nala
	1. Well, it's added, but I need to test/convert the script to work with Nala
2. Vesktop/Discord
#### Copy/Paste for Getting Started

```Shell
# Prepwork
mkdir Setup
cd ./Setup
touch ./FreshStart.sh
nano ./FreshStart.sh

## Set Permissions
chmod +x ./FreshStart.sh
sudo ./FreshStart

# Install Nala (included in script)
### NOTE: I observed some dependency issues if running on an unpatched system. Parrot OS broke, but Kali was fine.
wget https://gitlab.com/volian/volian-archive/uploads/b20bd8237a9b20f5a82f461ed0704ad4/volian-archive-keyring_0.1.0_all.deb
wget https://gitlab.com/volian/volian-archive/uploads/d6b3a118de5384a0be2462905f7e4301/volian-archive-nala_0.1.0_all.deb
sudo apt install ./volian-archive*.deb
echo "deb-src https://deb.volian.org/volian/ scar main" | sudo tee -a /etc/apt/sources.list.d/volian-archive-scar-unstable.list
sudo apt update && sudo apt install nala

# Reset virtual machine screen size
xrandr --output Virtual-1 --auto

## manually set Resilio user and group to current user
sudo nano /usr/lib/systemd/system/resilio-sync.service
sudo systemctl daemon-reload
sudo service resilio-sync restart 
```

#### Nala script
```Bash
# Get username
read -p "Enter username: " myuser

# Install installers and update APT
wget https://gitlab.com/volian/volian-archive/uploads/b20bd8237a9b20f5a82f461ed0704ad4/volian-archive-keyring_0.1.0_all.deb
wget https://gitlab.com/volian/volian-archive/uploads/d6b3a118de5384a0be2462905f7e4301/volian-archive-nala_0.1.0_all.deb
apt install ./volian-archive*.deb

echo "deb-src https://deb.volian.org/volian/ scar main" | sudo tee -a /etc/apt/sources.list.d/volian-archive-scar-unstable.list
apt update && apt install nala
nala install flatpak
nala install curl

# Resilio Sync
echo "deb http://linux-packages.resilio.com/resilio-sync/deb resilio-sync non-free" | sudo tee /etc/apt/sources.list.d/resilio-sync.list
wget -qO- https://linux-packages.resilio.com/resilio-sync/key.asc | sudo tee /etc/apt/trusted.gpg.d/resilio-sync.asc > /dev/null 2>&1
nala update
nala install resilio-sync -y
## Set to start (to create the service file) and run at startup
systemctl start resilio-sync
systemctl enable resilio-sync
## manually set Resilio user and group to current user
nano /usr/lib/systemd/system/resilio-sync.service
systemctl daemon-reload
service resilio-sync restart 

# Install Obsidian
nala install flatpak -y
flatpak install flathub md.obsidian.Obsidian

# Flameshot
nala install flameshot -y
systemctl enable flameshot

# KVM and Virt-Manager
nala install qemu-kvm libvirt-bin bridge-utils virt-manager virtinst virt-viewer -y

# DisplayLnk Drivers
wget https://www.synaptics.com/sites/default/files/Ubuntu/pool/stable/main/all/synaptics-repository-keyring.deb
nala install ./synaptics-repository-keyring.deb
nala update && nala install displaylink-driver -y

# 1Password
curl -sS https://downloads.1password.com/linux/keys/1password.asc | gpg --dearmor --output /usr/share/keyrings/1password-archive-keyring.gpg

echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/1password-archive-keyring.gpg] https://downloads.1password.com/linux/debian/amd64 stable main' | tee /etc/apt/sources.list.d/1password.list

mkdir -p /etc/debsig/policies/AC2D62742012EA22/
 curl -sS https://downloads.1password.com/linux/debian/debsig/1password.pol | tee /etc/debsig/policies/AC2D62742012EA22/1password.pol

mkdir -p /usr/share/debsig/keyrings/AC2D62742012EA22
 curl -sS https://downloads.1password.com/linux/keys/1password.asc | gpg --dearmor --output /usr/share/debsig/keyrings/AC2D62742012EA22/debsig.gpg

nala update && nala install 1password -y

# Tidal
flatpak install flathub com.mastermindzh.tidal-hifi


# General updates
nala update && nala upgrade -y
## nala sometimes misses upgrades, so running again with apt-get
apt update && apt upgrade -y
```

#### APT Script (DEPRECATED, no nala Modifications)
```Bash
# Get username
read -p "Enter username: " myuser

# Install installers and update APT
echo "deb-src https://deb.volian.org/volian/ scar main" | sudo tee -a /etc/apt/sources.list.d/volian-archive-scar-unstable.list
apt update && apt install nala
nala install flatpak

# Resilio Sync
echo "deb http://linux-packages.resilio.com/resilio-sync/deb resilio-sync non-free" | sudo tee /etc/apt/sources.list.d/resilio-sync.list
wget -qO- https://linux-packages.resilio.com/resilio-sync/key.asc | sudo tee /etc/apt/trusted.gpg.d/resilio-sync.asc > /dev/null 2>&1
apt-get update
apt-get install resilio-sync -y
## Set to start (to create the service file) and run at startup
systemctl start resilio-sync
systemctl enable resilio-sync
## manually set Resilio user and group to current user
nano /usr/lib/systemd/system/resilio-sync.service
systemctl daemon-reload
service resilio-sync restart 

# Install Obsidian
apt install flatpak -y
flatpak install flathub md.obsidian.Obsidian

# Flameshot
apt install flameshot -y
systemctl enable flameshot

# KVM and Virt-Manager
apt-get install qemu-kvm libvirt-bin bridge-utils virt-manager virtinst virt-viewer -y

# DisplayLnk Drivers
wget https://www.synaptics.com/sites/default/files/Ubuntu/pool/stable/main/all/synaptics-repository-keyring.deb
apt install ./synaptics-repository-keyring.deb
apt update
apt install displaylink-driver -y

# 1Password
apt install curl
curl -sS https://downloads.1password.com/linux/keys/1password.asc | gpg --dearmor --output /usr/share/keyrings/1password-archive-keyring.gpg

echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/1password-archive-keyring.gpg] https://downloads.1password.com/linux/debian/amd64 stable main' | tee /etc/apt/sources.list.d/1password.list

mkdir -p /etc/debsig/policies/AC2D62742012EA22/
 curl -sS https://downloads.1password.com/linux/debian/debsig/1password.pol | tee /etc/debsig/policies/AC2D62742012EA22/1password.pol

mkdir -p /usr/share/debsig/keyrings/AC2D62742012EA22
 curl -sS https://downloads.1password.com/linux/keys/1password.asc | gpg --dearmor --output /usr/share/debsig/keyrings/AC2D62742012EA22/debsig.gpg

apt update && sudo apt install 1password -y

# Tidal
flatpak install flathub com.mastermindzh.tidal-hifi

# General updates
apt update && apt upgrade -y
```

#### Links
[KVM hypervisor: a beginners’ guide | Ubuntu](https://ubuntu.com/blog/kvm-hyphervisor)


[KVM/VirtManager - Community Help Wiki](https://help.ubuntu.com/community/KVM/VirtManager)

[How to Install KVM and Create Virtual Machines on Ubuntu Desktop - LinuxBabe](https://www.linuxbabe.com/desktop-linux/how-to-install-kvm-and-create-virtual-machines-on-ubuntu-desktop)


### Othershit

```
## Linux babe
wget http://linux-packages.resilio.com/resilio-sync/key.asc
apt-key add key.asc
apt install software-properties-common
add-apt-repository "deb http://linux-packages.resilio.com/resilio-sync/deb resilio-sync non-free"
apt update
apt install resilio-sync
## Set to start (to create the service file) and run at startup
```