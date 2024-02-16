---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/udemy-chris-greer/s05-capturing-packets/"}
---

1. 3 kinds of packet capture
	1. Directly install Wireshark on the endpoint
		1. Pros
			1. Super easy
		2. Cons
			1. Often the machine is already stressed
			2. etc.
	2. SPAN/mirror port
		1. On a switch, mirror traffic to another interface with your device connected
		2. Easy to over-provision the port
	3. Network tap
		1. A physical device that sits on the wire and intercepts packets
			1. Couple hundred dollars, sometimes the client doesn't want the tap removed
2. Capturing at multiple locations
	1. Often useful when troubleshooting network slowness
	2. Ideal to get a capture on both sides of the connection (client and server)
3. Capture Filter (redux)
	1. If you really know what you're looking for, it can be helpful
		1. e.g., server side taps
	2. However, it's also easy to miss backend/alternate traffic that could be impacting the conversation
4. 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/guides/capture-traffic-with-wireshark/#start-capturing" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



### Start Capturing
1. Upon bootup, it will grab all interfaces and preview them
2. Click the blue shark fin at the top left, and it will begin capturing
3. Wireshark will only stop capturing when you manually stop capturing or system failure (loss of power, full storage, etc.)


</div></div>

5. 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/guides/capture-traffic-with-wireshark/#capture-options" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



### Capture Options
1. Capture>Options
	1. Input tab
		1. *Promiscuous mode* enables you to capture packets that are not destined for your machine
		2. Snaplength
			1. Normally set *default*, and you can set how much you capture
				1. Helpful if you are working in a secure environment, where you should not have access to the data in transit
				2. Also reduces pcap size


</div></div>

7. Long-term capture, intermittent, and cybersecurity issues
	1. Helpful for triaging intermittent issues or network threats
	2. 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/guides/capture-traffic-with-wireshark/#modify-output" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



### Modify Output
1. Capture>Options>Output
	1. Capture to a permanent file
		1. Name the file
		2. *Create a new file automatically...* to choose when a new file is created (recommended)
			1. Chris recommends doing it with *500* *megabytes* (about 50Gb), but you can choose increments based on packet count, size, and time
	2. Ring Buffer
		1. When creating a new PCAP file, it will delete the oldest PCAP in the series beyond the buffer


</div></div>

8. 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/guides/dumpcap/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




### Capturing packets via CLI with Dumpcap
1. Dumpcap super helpful on CLI-only environments
	1. Linux servers, etc.
2. Dumpcap on Windows
	1. Dumpcap gets installed with Wireshark along with several other tools
		1. `> cd C:\Program Files\Wireshark`
			1. rawshark.exe, tshark.exe, dumpcap.exe
	2. Add folder to path
		1. Control Panel>System>Advanced System Settings>Environment Variables
			1. Add path to Wireshark folder
3. Dumpcap commands
	1. `dumpcap -help`
		1. Shows list of all possible commands
	2. `dumpcap -D`
		1. List all available interfaces to capture from
	3. `dumpcap -i <interface number>`
		1. Begin dumpcap targeting the interface you want to capture
	4. `dumpcap -w <pcap file>`
		1. Enter the pcap file and directory you want to write to
	5. `dumpcap -b <ring-buffer options>`
		1. Configure the pcap options
			1. duration:NUM - switch to next file after NUM secs
			2. filesize:NUM - switch to next file after NUM kB
			3. files:NUM - ringbuffer: replace after NUM files
			4. packets:NUM - ringbuffer: replace after NUM packets
			5. interval:NUM - switch to next file when the time is an exact multiple of NUM secs
			6. printname:FILE - print filename to FILE when written (can use 'stdout' or 'stderr')

</div></div>
