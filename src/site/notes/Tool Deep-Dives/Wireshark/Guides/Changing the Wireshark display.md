---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/guides/changing-the-wireshark-display/","updated":"2024-02-14T11:54:50.000-08:00"}
---

## Configuring a profile
1. Create a new profile
	1. Right-click profiles at the bottom-right corner
	2. Select "New"
	3. Name the profile
		1. Unsure at this point if it's created based on the currently open profile or the Default.
2. Change the time
	1. View>Time Display Format
3. Adding Columns (including different time styles)
	1. Manually configuring a column
		1. Edit>Preferences>Appearance>Columns
			1. Alt, right-click a column header and select "Column Preferences"
		2. Click the `+` icon, name it, and then choose the information to be displayed
	2. Adding a column from packet information
		1. Right-click packet value
		2. Select *"Apply as column"*
4. Hiding/Removing Columns
	1. Right-click the column
		1. **To hide**: *Uncheck the box* next to the name
		2. **To delete**: Scroll to the bottom and choose *Remove this Column*
5. How to create custom filters
	1. Enter the filter you wish to use in the filter bar
		1. e.g., `tcp.flags.syn==1`
	2. click the `+` icon to the right of the bar
	3. Enter a name, and click "OK"
6. How to adjust traffic colors
	1. Through the packet
		2. Right-click the key value you wish to colorize
		3. Hover over "Colorize with Filter", then EITHER:
			1. Select a color to temporarily highlight all packets with similar values
				1. It can be reset with *Ctrl+Space* or *View>Colorize Conversation>Reset Colors*
			2. Select "New Coloring Rule" to bring up the preferences
	2. Through the settings
		1. View>Coloring Rules
		2. Click the `+` icon on the bottom left corner
		3. Add a description, and a filter, and choose the text (foreground) color and the background color

## Buttons to make your life easier
1. ![Changing the Wireshark display-2.png](/img/user/Attachments/Changing%20the%20Wireshark%20display-2.png)
	1. Start Capture
	2. Stop Capture
	3. Restart Capture
	4. Capture Settings
	5. Open PCAP file
	6. Save capture file
	7. Close capture file
	8. Reload capture file
	9. Find a packet
	10. Previous packet
	11. Next packet
	12. Jump to packet ![Changing the Wireshark display-1.png](/img/user/Attachments/Changing%20the%20Wireshark%20display-1.png)
	13. Go to top packet
	14. Got to last packet
	15. Automatically scroll to most recent packet during live capture
	16. Enable/Disable color filtering rules
	17. Increase window text size
	18. Decrease window text size
	19. Return text size to default
	20. Shrink/Expand columns to fit details