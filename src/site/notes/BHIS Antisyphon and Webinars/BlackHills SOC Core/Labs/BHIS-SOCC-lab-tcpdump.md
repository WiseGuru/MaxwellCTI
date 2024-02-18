---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/bhis-socc-lab-tcpdump/"}
---

 [TCPdump Labs](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/TCPDump/TCPDump.md)


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/tcpdump/#tcpdump" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### tcpdump
- Unix/Linux CLI-based packet capture and analyzer utility that is extremely flexible
	- Also fun fact, the `man` page for `tcpdump` breaks down the [TCP header](https://ccnadefinitions.com/ccna/20-definitions/tcp/) 
- Not as feature-right as [[Tool Deep-Dives/Wireshark\|Wireshark]], but can run in more places with less overhead more quickly
- Important commands
	- `-D`
		- Lists interfaces
	- `-XA`
		- X is for Hex
		- A is for ASCII
	- `-w`
		- Write the data to a file
	- `-n`
		- Don't resolve hostname
	- `-r <file>`
		- Identify the file you're reading from
- Like Wireshark, you can also filter output by net, host, and/or port
	- `tcpdump host 192.168.1.2 port 80`
	- `tcpdump net 192.168.1.0/24 port 445`
- **CRITICALLY**, the output can be managed more easily when piped to various Linux commands
	- [[Tool Deep-Dives/less\|less]]
	- [[Tool Deep-Dives/grep\|grep]]
		- Specifically, you can use `\|` to grab multiple variables, like `grep '3389\|445\|CLOSED\|ESTABLISH'
- **Flags**
	- [reading tcp flags](https://gist.github.com/tuxfight3r/9ac030cb0d707bb446c7)



</div></div>


## The tcpdump Lab
 [TCPdump Labs](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/TCPDump/TCPDump.md)


So first we cd to the folder with the PCAP, open it, and sort by the host machine.
- `tcpdump -n -r magnitude_1hr.pcap host 192.168.99.52`
	- `-n` 
		- Instructs tcpdump not to resolve the hostnames
		- This is key, because otherwise it stops every couple seconds to resolve domain names
	- `-r <file>` 
		- Identifies the file to modify
		- **NOTE**: It is helpful, nay necessary, to spell the filename correctly.
			- You can enter the first letter of the file and then press *tab* to autofill the file name

Many of the entries in the PCAP are broken up roughly like this:

`<time> <protocol> sourceip.protocol>targetip.port: flags seq# ack# <window size> <packet length> <packet contents protocol>`

Anyway, lots here to unpack, but also a lot of chaff to sort through, so we'll filter further. Tack on `and port 80` to filter only traffic through port 80 for HTTP traffic (or any traffic going over port 80).

You can also string ports together using `or`, for example, `tcpdump -n -r magnitude_1hr.pcap host 192.168.99.52 and port 443 or 80`.

We were already seeing a bunch of HTTP packets, so to help get a clearer picture of what's going on, we can add `-A` to show the packet contents, and see what data is being transmitted. The output blows past, but we can pipe the output to `less` to make it easier to scroll.

`tcpdump -n -r magnitude_1hr.pcap host 192.168.99.52 and port 443 or 80 | less`

In the first fill screen, we see that PowerShell is being sent from an external IP to our host private IP address, and it's encoded in [[Tool Deep-Dives/Base64\|Base64]] to obfuscate the malicious code. Bad-bad!

> You can also filter by protocol; in the lab, they append `ip6` to the command to show only IPv6 entries, but you could filter by `arp`, `stp`, but not `lldp` for some reason.

