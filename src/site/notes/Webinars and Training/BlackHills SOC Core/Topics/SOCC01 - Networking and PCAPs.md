---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/topics/socc-01-networking-and-pca-ps/","noteIcon":""}
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
| Port | Protocol           | Interesting Links                                                                                                                       |
| ---- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| 80   | HTTP               |                                                                                                                                         |
| 23   | Telnet             |                                                                                                                                         |
| 22   | SSH                |                                                                                                                                         |
| 443  | HTTPS              |                                                                                                                                         |
| 3389 | ms-term-serv (RDP) |                                                                                                                                         |
| 445  | microsoft-ds (SMB) | SMB is synonymous in my head with EternalBlue [EternalBlue - Wikipedia](https://en.wikipedia.org/wiki/EternalBlue)                      |
| 139  | netbios-ssn        | [137,138,139 - Pentesting NetBios - HackTricks](https://book.hacktricks.xyz/network-services-pentesting/137-138-139-pentesting-netbios) |
| 21   | FTP                |                                                                                                                                         |
| 135  | MSRPC              | [Microsoft RPC - Wikipedia](https://en.wikipedia.org/wiki/Microsoft_RPC)                                                                |
| 25   | SMTP               |                                                                                                                                         |
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

<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">





</div></div>


### Lab: [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-tcpdump\|tcpdump]]

## Wireshark Lab
We then did a [[Tool Deep-Dives/Wireshark\|Wireshark]] lab, but there really wasn't anything new compared to the [[Tool Deep-Dives/Wireshark/Udemy - Chris Greer/S00 - Course Overview\|Wireshark Udemy course with Chris Greer]] that I took earlier, so I didn't take any notes at the time. There were two key takeaways though that I forgot or wasn't covered in the Udemy course:


<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">





</div></div>


