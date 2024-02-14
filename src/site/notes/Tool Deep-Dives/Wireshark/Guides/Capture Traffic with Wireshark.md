---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/guides/capture-traffic-with-wireshark/"}
---

### Start Capturing
1. Upon bootup, it will grab all interfaces and preview them
2. Click the blue shark fin at the top left, and it will begin capturing
3. Wireshark will only stop capturing when you manually stop capturing or system failure (loss of power, full storage, etc.)

### Capture Options
1. Capture>Options
	1. Input tab
		1. *Promiscuous mode* enables you to capture packets that are not destined for your machine
		2. Snaplength
			1. Normally set *default*, and you can set how much you capture
				1. Helpful if you are working in a secure environment, where you should not have access to the data in transit
				2. Also reduces pcap size

### Modify Output
1. Capture>Options>Output
	1. Capture to a permanent file
		1. Name the file
		2. *Create a new file automatically...* to choose when a new file is created (recommended)
			1. Chris recommends doing it with *500* *megabytes* (about 50Gb), but you can choose increments based on packet count, size, and time
	2. Ring Buffer
		1. When creating a new PCAP file, it will delete the oldest PCAP in the series beyond the buffer

### Notes on Captured Traffic
1. Traffic captured on devices might not be segmented
	1. e.g., a packet might look too big, because it hasn't been broken up yet for transport.