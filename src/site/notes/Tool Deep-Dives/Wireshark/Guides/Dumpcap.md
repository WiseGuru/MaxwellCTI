---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/guides/dumpcap/"}
---

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