---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/udemy-chris-greer/s7-practical-ip-analysis/"}
---

1. IP Identification
	1. Sometimes, the starting point can indicate the operating system in use
	2. If the ID's are incrementing in large numbers, it could indicate the sender is busy with other hosts
		1. Windows 11 seems to increment by default
	3. Some developers/servers just use ID of **0**
2. IP Fragmentation
	1. Packet sizes are typically limited to 1500 bytes
		1. Larger packets can be fragmented to allow transmission
	2. Characteristics of fragmented packets
		1. They share IP identification numbers
		2. The first packet is marked as "fragmented" with the `ip.flags.mf` (*more fragments*)  bit flipped to 1
		3. Subsequent packets are marked with the `ip.frag_offset` flags that mark how much of an offset there is from the packet immediately preceding it
			1. First packet should be 0, second packet might something like 1480, etc.
	3. In the far-left column in Wireshark, if there are dots, it indicates that the packets were reassembled/part of a larger whole
		1. ![S7 - Practical IP Analysis-1.png](/img/user/Attachments/S7%20-%20Practical%20IP%20Analysis-1.png)![S7 - Practical IP Analysis-2.png](/img/user/Attachments/S7%20-%20Practical%20IP%20Analysis-2.png)
3. IP Flags
	1. **Don't Fragment**
		1. most TCP packets have this bit set
		2. Often, routers will drop them, and respond with an ICMP packet indicating it was dropped due to size
4. NMAP scan
	1. NMAP scans tend to be small
		1. The initial scan should just be about 24 bytes, might be scattered over different fragments
			1. e.g., one packet gets fragment into three 8-byte-long payloads (with a total length of 28 bytes)
	2. Packets are fragmented in order to slip past some IDS/IPS/Firewalls, because the fragmented data is not always understandable by the security hardware
5. IPv6 packets
6. 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/wireshark/guides/geo-ip-databases/#how-to-add-geo-ip-information-to-wireshark" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



## How to add GeoIP information to Wireshark
1. Download and extract the databases from MaxMind (great name)
	1. Have to sign up for a free account and GeoLite 2 license
	2. Navigate to Downloads, and download the ASN, City, and Country databases in GZIP (not the CSV format versions)
		1. I have them saved/extracted to `C:\Users\user\Documents\Wireshark\MaxMind GeoIP Databases`
	3. You have to extract them not just from the GZ, but also from the TAR
2. Activate them in Wireshark
	1. Edit>Preferences>Name Resolution>MaxMind database directories

</div></div>

7. Activity
	1. How many unique IP hosts do we see in this pcap?
		1. There are 5868 endpoints
			1. Statistics>Endpoints
	2. How many packets are in the top IP conversation?
		1. 32
			1. Statistics>Conversations>IPv4, sort by packets
	3. What country is the 62.189.238.32 endpoint communicating from? (Full country name)
		1. United Kingdom
	4. Can you work out how to filter the hosts coming from Turkey? How many packets do you find after filtering for Turkey? (hint: `ip.geoip.country_iso==?`)
		1. 389
			1. `ip.geoip.country == "Turkey"` or `ip.geoip.country_iso == "TR"`
	5. Look at packet 1 - what is the IP TTL for this packet?
		1. 123
	6. What is the IP Identification number for this packet?
		1. 256
	7. Look at packet 7, which appears to be coming from the same subnet. What is the IP Identification number?
		1. 256
	8. Add IP TTL as a column and filter for all packets coming FROM the 212.252.0.0/16 subnet ranges. (Enter OK as the answer)
		1. `ip.src >= 212.252.0.0 and ip.src <=212.252.255.255`
		2. `ip.src wq 212.252.0.0/16`
	9. Does the IP TTL change for any of these packets? Y/N
		1. No
	10. Add the IP ID as a column. Does the IP ID change for any of these packets? Y/N
		1. No
	11. Do you think these packets could be spoofed IP's? Y/N
		1. Yes