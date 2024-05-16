---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/disable-windows-memory-integrity/"}
---

Disabling Windows Memory Integrity is simple enough, and necessary if you're running a VM within a VM^[YoDawgIHerdULike.jpeg] and you have a few ways of getting there.
1. Settings
	1. Open Settings and select "Privacy & security" in the left column
	2. Choose "Device Security", then select "Core isolation details"
	3. Toggle-off the option below "Memory integrity"
2. Windows Defender
	1. Click on "Windows Defender" in the system tray
	2. Choose "Device Security", then select "Core isolation details"
	3. Toggle-off the option below "Memory integrity"

But once you're done and want to re-enable Memory Integrity, you might get an error about drivers that are incompatible with it. It's frankly a little absurd that often these drivers have been on the system since the computer was imaged with Windows... wtf?

Anyway, here's how I solved the problem quickly and easily, without having to download anything dangerous.

> **NOTE**: The screenshots below were taken from [this video by TechMens Land](https://www.youtube.com/watch?v=5_IBEGLRdLI), because by the time I realized I should document my process, I'd solved the problem.

1. Click on the "Review incompatible drivers" link from the "Memory Integrity" management page.
2. Click on the listed drivers to expand their details.
	1. Under details, you should see information like Import date, driver date, etc., but we care about the "Published name"^[An example of what you're looking for: ![Disable Windows Memory Integrity-1.png](/img/user/Attachments/Disable%20Windows%20Memory%20Integrity-1.png)]
	2. **NOTE**: while investigating, you will find the same driver listed as `example.sys`,^[I'd written a whole footnote calling out my friend Kevin for probably getting mad at me for not following AP Style by putting commas outside of the quotes in order to indicate an exact string of characters, but then I realized I could just use the code-block formatting to accomplish the same thing without the weird formatting or upsetting Kevin, and I just wanted someone, anyone, to know of this struggle.] `example.inf`, and `oem420.inf`; this is normal.
		1. `example.sys` is the *actual* driver itself which contains the critical information
		2. `example.inf` contains the *information* that identifies it to the system and describes the driver
		3. `oem420.inf` is the *device specific* name for the driver, based on the order of installation.
			1. This helps avoid problems if there are two drivers named `example.inf` but published by two different entities and that are otherwise unrelated
3. Run Powershell or CMD as an administrator, and use `pnputil` to uninstall and remove the offending drivers, identifying them by their *Published name* (e.g., `oem420.inf`)
	1. `pnputil /delete-driver <Published Name> /uninstall /force`
4. Once you remove the incompatible drivers, go back to the "Core Isolation" page and ​select "Scan Again"
	1. Memory Integrity should be re-enabled, and you just need to reboot to finish applying the changes.