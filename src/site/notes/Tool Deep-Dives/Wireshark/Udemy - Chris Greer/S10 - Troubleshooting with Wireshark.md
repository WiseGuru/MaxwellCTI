---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/udemy-chris-greer/s10-troubleshooting-with-wireshark/"}
---

1. Slow application response time
	1. First thing; look at network round-trip time (SYN, SYN-ACK delta)
	2. Second: Look for transmission of data, and compare
		1. In a Layer 4 protocol (HTTP, SMB, etc.), Wireshark can quickly show time since request (http.time, smb.time, etc.)
	3. If delay is coming from client, it's possible the issue is Layer 8 (client not clicking "OK," "Next," etc,), and stalling the application, and not an issue with the service or app
2. High network latency
	1. First thing; check network round trip time
	2. 40 milliseconds is slow
		1. A couples times is no big deal, but they add up
		2. Right-click packet: *Set/Unset Time Reference* to set it as the baseline
			1. ![S10 - Troubleshooting with Wireshark-1.png](/img/user/Attachments/S10%20-%20Troubleshooting%20with%20Wireshark-1.png)
	3. Not just that it happens, but how often it happens
3. Network packet loss
	1. Search for "TCP Previous segment not captures" with `tcp.analysis.lost_segment`
	2. Lots of packet loss can indicate an issue on the network; bad router/switch, cabling, etc.
4. Slow file transfers - TCP Window Problems
	1. Communicating Available Buffer size
		1. Client Sends a packet with its *calculated window size*, basically it's available buffer space
		2. Server sends as much data as it can
			1. If the data sent matches the *calculated window size*, Wireshark uses an analysis flag indicating that the client **TCP Window (is) Full**
			2. The client will then respond with *Window* value of **0**, indicating that its buffer is full
			3. Once the buffer is cleared, it will send a new packet with its new window size
			4. ![S10 - Troubleshooting with Wireshark-2.png](/img/user/Attachments/S10%20-%20Troubleshooting%20with%20Wireshark-2.png)
	2. In this example, the issue was client side; *it was unable to accept new data until it processed its buffer*
5. Network/Application Disconnects
	1. First thing: Think about *resets*
		1. TCP isn't able to get the connection off the ground
	2. Example: Existing conversations work, new conversations fail
		1. 3 SYNs, 2 resets, one SYN-ACK
		2. Check for other resets
			1. Looks like a bunch of resets are going on with this client in particular
		3. Check the Reset packet TTL distance
			1. Did it actually come from the endhost?
				1. SYN: 128
					1. Unrouted, originating from the client
				2. Reset 1: TTL 64
					1. Often means it's *unrouted*, possibly originating from a Firewall
				3. SYN-ACK: 244
					1. Probably decrementing from 255, so 11 hops away
			2. It looks like the Reset is coming from a different device on the network
				1. Confirm by checking the Source MAC address
					1. In this case, it showed a Sonicwall Firewall as the source.
6. Home practice
	1. Analyze home network traffic
		1. How long does it take to reach app servers?
		2. Common network latency?
		3. DNS response time?
		4. What's the TTL of your application servers?
		5. Are you using IPv4 or IPv6?
		6. Are the apps really chatty?