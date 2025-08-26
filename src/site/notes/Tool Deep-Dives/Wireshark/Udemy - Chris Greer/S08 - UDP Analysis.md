---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/udemy-chris-greer/s08-udp-analysis/","updated":"2024-06-02T11:24:25.000-07:00"}
---

1. UDP Header
	1. 8-bytes only
		1. Source, Dest, Length, Checksum 
2. DHCP process
	1. DORA
		1. *Discover*/*Request*
			1. MAC
				1. source: (client MAC), Dest: F:F:F:F:F:F
			2. IP
				1. Source: 0.0.0.0, Dest: 255.255.255.255
		3. *Offer*/*Acknowledge*
			1. MAC
				1. Source: (router MAC), Dest: (client MAC)
			2. Source: 192.168.0.1, Dest: 192.168.0.10
			3. Includes Relay agent address if used
	2. DHCP Flags
		1. Options 50, 53, 55, etc.
3. DNS
	1. Frequently there are lots of DNS requests, as sites pull different information
	2. You can check DNS response times with `dns.time>.04`
4. VoIP Analysis
	1. Check the delta time between packets
		1. Anything greater than 0.03 seconds can lead to jitter
			1. Lower is better
	2. RTP Sequence Numbers
		1. Missing numbers in the sequence means dropped packets
	3. IP Differentiated Services Codepoint
		1. Expedited Forwarding is what it should be
			1. If it's set to default, the packets won't be treated with priority



<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">




#### UDP
- *UDP* (*User Datagram Protocol*) is a connectionless communication protocol that allows data to be sent without establishing a connection, providing low latency and minimal overhead at the cost of reliability.:






# Metadata

### Sources


### Tags
#defs_sec 

</div></div>
 


