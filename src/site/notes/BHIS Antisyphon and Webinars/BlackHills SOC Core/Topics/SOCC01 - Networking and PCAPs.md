---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/topics/socc-01-networking-and-pca-ps/"}
---

## IP/TCP Headers and Ports
- General discussion of the IP header
	- Routers predominately are the devices that only look at the IP Header
- IP Header
	- [BGP Hijacking](https://www.cloudflare.com/learning/security/glossary/bgp-hijacking/)
		- APT/Nation-state actor can compromise an [ASN](https://www.cloudflare.com/learning/network-layer/what-is-an-autonomous-system/) and redirect traffic to another destination
		- *Traffic is routed to the most specific address*
	- Fragmentation flags
		- x - bit 0
			- ["the evil bit"](https://en.wikipedia.org/wiki/Evil_bit)
			- Reserved, but be 0
		- D (DF) - bit 1
			- Don't fragment
		- M (MF) - bit 2
			- More Fragments
- TCP Header
	- Describes the ports for the packets
		- Ports are *generally* used by certain protocols, but *generally*, ports can be used by any service
		- Port **0** can be used, and has been used as a 
	- [NAT/PAT](https://ccnadefinitions.com/ccna/20-definitions/nat/)
	- RFC 793 describes the 3-way handshake
		- Originally, it was a 4-way handshake, but most modern OS's send a 3-way instead.
### OWASP Top 10 ports
| Port | Protocol | Interesting Links |
| ---- | ---- | ---- |
| 80 | HTTP |  |
| 23 | Telnet |  |
| 22 | SSH |  |
| 443 | HTTPS |  |
| 3389 | ms-term-serv (RDP) |  |
| 445 | microsoft-ds (SMB) | SMB is synonymous in my head with EternalBlue [EternalBlue - Wikipedia](https://en.wikipedia.org/wiki/EternalBlue) |
| 139 | netbios-ssn | [137,138,139 - Pentesting NetBios - HackTricks](https://book.hacktricks.xyz/network-services-pentesting/137-138-139-pentesting-netbios) |
| 21 | FTP |  |
| 135 | MSRPC | [Microsoft RPC - Wikipedia](https://en.wikipedia.org/wiki/Microsoft_RPC) |
| 25 | SMTP |  |
### Shodan top ports
| Port | Protocol | Notes |
| ---- | ---- | ---- |
| 80, 8080, 443, 8443 | HTTP/S |  |
| 21 | FTP |  |
| 22 | SSH |  |
| 23 | Telnet |  |
| 161 | SNMP |  |
| 143, 993 | IMAP/Encrypted |  |
| 25 | SMTP |  |
| 5060 | SIP |  |
| 554 | RTSP (Real Time Streaming Protocol) |  |
## tcpdump

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



<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/bhis-socc-lab-tcpdump/#the-tcpdump-lab" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



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


</div></div>


## Wireshark
We then did a [[Tool Deep-Dives/Wireshark\|Wireshark]] lab, but there really wasn't anything new compared to the [[Tool Deep-Dives/Wireshark/Udemy - Chris Greer/S00 - Course Overview\|Wireshark Udemy course with Chris Greer]] that I took earlier. There were two key takeaways though that I forgot or wasn't covered in the Udemy course:


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/#wireshark-hot-tips" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Wireshark Hot Tips
- **KEY**: In a conversation, right click a packet and select **Follow>TCP Stream** to show the *data that is being sent between hosts*  🤯
- **KEY**: you can right-click, select **HTTP>Requests** to see *the malicious crap that's flying around*





</div></div>

