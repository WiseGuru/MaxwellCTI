---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/udemy-chris-greer/s04-filtering-traffic/"}
---

1. Introduction
	1. Common Wireshark Filters 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/guides/wireshark-filters/#common" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



## Common

| Filter type         | Display Filter (example) |
| ------------------- | ------------------------ |
| IPv4 Address        | `ip.addr==10.0.0.1`      |
| IPv4 Source         | `ip.src==10.0.0.1`       |
| IPv4 Range (Subnet) | `ip.addr==10.0.0.0/24`   |
| TCP Port            | `tcp.port==80`           |
| TCP SYNs            | `tcp.flags.syn==1`       |
| Protocol Filters    | `arp`, `udp`, etc.       |
|                     |                          |


</div></div>

	2. When filtering traffic with filters, the filter bar will be GREEN when correct, RED when there's something wrong, or YELLOW when it could be ambiguous
2. Capture Filters vs Display Filters
	1. *Display filters* are applied after the fact, while investigating captured packets
	2. A *capture filter* is applied when you're first bringing packets into the device
		1. Only want to focus on a certain IP, etc.
		2. Much simpler than display filters
			1. Built using the [[Tool Deep-Dives/Wireshark/Berkeley Packet Filter Syntax\|Berkeley Packet Filter Syntax]]
		3. Not as ideal as *Display filters*, because you're more likely to miss what you're actually looking for
3. Filtering Traffic
	1. You can filter packets by protocol, but sometimes it's worth going deeper
		1. e.g., filtering by `http` will only reveal packets that include HTTP payloads, but not the three-way [TCP](https://ccnadefinitions.com/ccna/20-definitions/tcp/) handshake at the beginning
			1. Filtering with `tcp.port==80` WILL show the handshake, and any other traffic going over TCP port 80, which should only be HTTP and other related protocols
4. Filtering by Conversation
	1. Think about the basis of your conversation
		1. MAC address? IP? TCP sequence?
	2. Right-click>Apply as Filter, Conversation Filter, etc.
5. Modifiers
	1. There are common modifiers used by wireshark
	2. You can use the "English" or shortcut version interchangeably
	3. 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/guides/wireshark-filters/#wireshark-modifiers" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



## Wireshark Modifiers

| Modifier       | English | Shortcut                                          |
| -------------- | ------- | ------------------------------------------------- |
| Equals         | eq      | ==                                                |
| Does not Equal | not     | !                                                 |
| Or             | or      | (double bars like ll)                             |
| And            | and     | &&                                                |
| Greater than   | gt      | >                                                 |
| Less than      | lt      | <                                                 |
| Set as object  | ( )     | e.g., `!(eth.type eq 0x0800)` means NOT type 0x0800 |

**NOTE**: Using l instead of | for Obsidian formatting



</div></div>

6. Save As vs Export
	1. "Save As" saves everything, filtered or not
	2. "Extract Specified Packets" allows you to choose which packets you want to export
7. Activity
	1. How many DNS packets are in the trace file?
		1. Couple of ways to search: `dns`, `udp.port==53`
		2. 228 DNS packets
	2. How many DNS packets contain the word "Udemy"? (Regardless of case)
		1. `dns matches "udemy"`
		2. 20
	3. How many HTTP packets are in the pcap?
		1. `http` or `tcp.port==80`
		2. **66** with `http`, 211 with TCP
	4. Set a filter for TCP port 80. How many packets meet that filter?
		1. `tcp.port==80`
		2. 211
	5. How many packets are in the top IP conversation? Set a filter for this conversation.
		1. OH, it's asking about the conversation with the most packets
			1. Statistics>Conversations>IPv4>Sort by Packets
		2. 406
	6. In the top IP conversation, how many packets have the word "Udemy", regardless of case?
		1. `(ip.addr==10.0.2.15 && ip.addr==104.16.65.85) and frame matches "udemy"`
		2. 3
	7. How many packets have the SYN bit set?
		1. Select, find SYN flag, Right-click>Apply as Filter>Selected
		2. 146
	8. How many TCP Resets are in the pcap?
		1. `tcp.flags.reset==1`
		2. 9
	9. How many TCP SYN/ACKs are in the pcap?
		1. `tcp.flags.syn==1 and tcp.flags.ack==1` or `tcp.flags==0x012`
		2. 73
	10. Are any SYN/ACKs coming from the 10.0.2.15 station? Y/N?
		1. `tcp.flags.syn==1 and tcp.flags.ack==1 && ip.src==10.0.2.15`
		2. No