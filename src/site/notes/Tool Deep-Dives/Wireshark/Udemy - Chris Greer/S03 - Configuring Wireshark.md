---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/udemy-chris-greer/s03-configuring-wireshark/"}
---


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/guides/changing-the-wireshark-display/#configuring-a-profile" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



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


</div></div>



## Exercise
1. Add a coloring rule that will make your tcp FIN packets blue. What filter will you use to do that?
	1. tcp.flags.fin == 1
	2. Colorize Conversation
3. Select packet number 1. Can you find the TCP segment length? Add this value as a column. Enter "done" in the answer field below when finished.
	1. Done
4. It would be nice to have a button that quickly filters for all TCP Errors. See if you can find the TCP Retransmission we were looking at earlier. How can you filter for all TCP errors in the trace file? What is this filter?
	1. tcp.analysis.flags
5. Add the TCP Errors filter as a button in this profile. Enter "done" below when finished.
	1. Added the button by pressing the `+` icon next to the filter field, and then giving it a name
	2. Done
7. It can be a little overkill to see timestamps all the way to the nanosecond. Using the View | Time Display Format menu option, can you see how to configure Wireshark to only display to the microsecond? Make this change in this profile and type "done" below.
	1. Done
