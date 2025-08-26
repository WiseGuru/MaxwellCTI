---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/udemy-chris-greer/s01-02-introduction/","updated":"2024-03-09T11:58:22.000-08:00"}
---

## First Exercise
1. How many packets were captured in this trace file?
	1. 2186
2. What protocol does packet number 8 contain? (The highest-layer protocol)
	1. HTTP
3. If you just installed Wireshark for the first time, what is the name of the profile you are using? (bottom right corner)
	1. Default
4. Look at packet number one - what is the source IP address in this packet?]
	1. 192.168.56.102
5. What is the source TCP port in this same packet?
	1. 39294
6. What TCP flag is set in this packet?
	1. SYN
7. What is the frame number of the next packet in this TCP conversation?
	1. *6 (for Wireshark)*
	2. 0 (for relative sequence number)
	3. 1949477806 for the raw sequence number
8. Can you set a filter for this TCP conversation? How many packets do you get?
	1. Yes
		1. To set a conversation filter, right-click the packet, select *conversation filter* from the list, and choose *TCP* to indicate you want to only select this TCP conversation
			1. Could also do by Ethernet or IPv4
	2. 51 packets displayed

