---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/udemy-chris-greer/s06-anatomy-of-a-packet/","updated":"2024-02-16T13:36:19.000-08:00"}
---

1. OSI and TCP/IP model review
2. Ethernet - The Frame Header
	1. OSI Layer 2 and TCP/IP Layer 1
	2. Format: Destination MAC, Source MAC, Ethertype, (Data, etc.), FCS
		1. Wireshark doesn't show the FCS or Preamble
	3. You will also see multicast, broadcast, and unicast frames
3. Network Layer Packet
	1. OSI Layer 3 - IPv4 and IPv6
	2. Format: [Packet](https://ccnadefinitions.com/ccna/20-definitions/packet/)
4. Assignment
	1. Focus on packet number two and answer the following questions. What is the destination MAC address? (Use format xx:xx:xx:xx:xx:xx)
		1. 00:00:0c:0c:00:0e
	2. What is the source MAC in this packet?
		1. 00:0c:29:65:3b:25
	3. What is the IP identification number? (Use format xxxxxx with no commas)
		1. 32371 (or 0x7e73)
	4. What is the IP Time To Live?
		1. 128
	5. What is the Source IP?
		1. 192.168.1.10
	6. Now change pcaps to the server side. Open udemy-server-slowfiledownload.pcapng. Which packet corresponds to the packet we were analyzing on the client side? What is the frame number?
		1. Frame 11 
	7. Focus on this frame. What is the source MAC?
		1. 00:00:0c:9c:00:ff
	8. What is the destination MAC?
		1. 00:06:5b:00:02:ff
	9. What is the IP ID?
		1. 32371
	10. What is the IP TTL?
		1. 127
	11. How many routers did this packet go through?
		1. 1
	12. Was there a NAT (Network Address Translation) along the path? Y/N
		1. No