---
{"dg-publish":true,"permalink":"/tool-deep-dives/pf-sense/"}
---

If you use pfSense as your router and configure Tailscale on it, delete the FUCK out of the interface it creates before you do anything else.

When the router reboots, it will fail to load the tailscale virtual interface, dump all connected interfaces, and you WILL NOT be able to get back into the GUI and you WILL have to connect to the router with a screen and keyboard to configure the interfaces, and any other (working) virtual interfaces will not be configurable, so any physical interfaces that come after the virtual ones won't be configured, and then you'll have to download recent backup config, remove the tailscale interface, reload the config, reboot, and then reconfigure the interfaces in the right order so that everything loads correctly because naming the interface doesn't matter; the hardcoded name like OPT5 or OPT6 aren't just placeholders, they are how the system handles them, and even if you flip their names, the system will think of their hardcoded position only, so your firewall and routing configs will be screwed until EITHER you modify them to use the new interface orientation, which is a long and tedious process, or you recreate the interfaces in the correct order and hope it works. 






# Metadata

### Sources

### Tags
#tools_sec 