---
{"dg-publish":true,"permalink":"/technical-guides/installing-the-zen-browser/","updated":"2025-02-20T13:09:13.478-08:00"}
---

>Written 2025.02.20

I wanted to try out the [Zen Browser](https://zen-browser.app/) on Linux, but discovered that it's not available in a repository. So I downloaded the x86 Tarball, because I didn't want to use either Flathub or AppImage to manage it. Here are the steps I took to install it.

# Steps to install
1. Extract the Tarball and inspect the files
	1. It extracted to a `zen` directory into my Downloads folder, and it didn't have any of the typical installation files or references on moving forward; it looked more like an application directory.
		1. I was looking for README, INSTALL, configure, etc., but none of these existed, and there were no files at the same level of the `zen` folder
	2. I navigated to the folder in a terminal and ran `zen`, and lo and behold, the browser opened!
		1. However, I found that I could not pin the application to the task bar, so I would need to look into that.
2. Move the folder to an application folder
	1. Rather than move the browser to a shared location, I created the folder `~/apps`^[As a reminder, the `~` indicates my home folder.] and copied `zen` into it.
	2. To make it so that I could open the application from any terminal, I created a link to the application in the [[Tool Deep-Dives/Linux/PATH Variable\|PATH Variable]] using `ln ~/apps/zen/zen ~/bin/zen`
3. Create a desktop entry file to allow pinning in my desktop environment
	1. Create and edit the desktop file in your profile's `.local` applications folder: `nano ~/.local/share/applications/zen-browser.desktop`
	2. Added the information [[Technical Guides/Installing the Zen Browser#zen-browser.desktop\|found below]]
	3. Make the file executable: `chmod +x ~/.local/share/applications/zen-browser.desktop`
	4. It worked for me immediately, but if it doesn't work for you, log out, then log back in again.

After all that, I decided I like Firefox better. I guess I'm still not ready to move my tabs to a side column ¯\\\_(ツ)\_/¯

### zen-browser.desktop

```bash
[Desktop Entry]
Version=1.0
Name=Zen Browser
Comment=Browse the web with Zen Browser
Exec=/home/max/apps/zen/zen %U
Icon=/home/max/apps/zen/browser/chrome/icons/default/default128.png
Terminal=false
Type=Application
Categories=Network;WebBrowser;
```