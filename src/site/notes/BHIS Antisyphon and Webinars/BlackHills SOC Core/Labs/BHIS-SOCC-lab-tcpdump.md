---
{"dg-publish":true,"permalink":"/bhis-antisyphon-and-webinars/black-hills-soc-core/labs/bhis-socc-lab-tcpdump/"}
---

 A quick reminder on tcpdump before we get started:


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
	- [[Tool Deep-Dives/Linux/less\|less]]
	- [[Tool Deep-Dives/Linux/grep\|grep]]
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

# Bonus Lab
[Malware of the Day - What Time Is It? - Active Countermeasures](https://www.activecountermeasures.com/malware-of-the-day-what-time-is-it/)
Now that we have the basics, let's take a look at the most recent^[At the time of writing, 2024/02/19] Malware of the Day (linked at the bottom of the tcpdump lab).

I download the 24-hour PCAP file and navigate to it using the Ubuntu shell.

```Shell
cd /mnt/c/users/adhd/Downloads
tcpdump -n -r timeservsync_24r.pcap
```

So this PCAP comes from a networking device (and not a host), which we can tell because it lists traffic between an various public IPs and various Private (i.e., 192.168.0.0/16) addresses.

I haven't looked at the preamble for this malware, so let's start with the OWASP Top 10 ports (ignoring HTTP/S for now)


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/bhis-antisyphon-and-webinars/black-hills-soc-core/topics/socc-01-networking-and-pca-ps/#owasp-top-10-ports" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



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

</div></div>


`tcpdump -n -r timeserversync_24hr.pcap net 192.168.0.0/16 and port 23 or 22 or 3389 or 445 or 139 or 21 or 135 or 25`

No hits, so let's check the HTTP traffic next.

`tcpdump -n -r timeserversync_24hr.pcap net 192.168.0.0/16 and port 80 or 443 -A | less`

I'm just scrolling through the beginning of the capture, and I'm noting a pattern of DOWNGRD in the HTTPS packets; this makes me think it might be a [Downgrade attack](https://www.crowdstrike.com/cybersecurity-101/attack-types/downgrade-attacks/), but I'm not sure.

Let's take a closer look at that server.

`tcpdump -n -r timeserversync_24hr.pcap host 157.245.128.27 | less`

![BHIS-SOCC-lab-tcpdump-1.png](/img/user/Attachments/BHIS-SOCC-lab-tcpdump-1.png)

1. Host `192.168.99.51` is checking in constantly with `157.245.128.27`
	1. First three packets are the [[Tool Deep-Dives/Wireshark/Guides/TCP Details\|TCP 3-Way Handshake]]
	2. Some data is exchanged
	3. The connection terminates
2. Adding `-XA` the command shows that the domain is timeserversync(.)com
	1. However, NTP is a UDP protocol, not TCP, and is transmitted over port 123
	2. Checking virustotal.com, timeserversync(.)com is not found to be malicious, but the IP address `157.245.128.27` is flagged as malicious with a Powershell script named `dump1.ps1`
 ![BHIS-SOCC-lab-tcpdump-3.png](/img/user/Attachments/BHIS-SOCC-lab-tcpdump-3.png)

If we search the network for the NTP port, we find that there is a preconfigured NTP server, and host `99.51` is connecting with it with the NTPv3 protocol
![BHIS-SOCC-lab-tcpdump-2.png](/img/user/Attachments/BHIS-SOCC-lab-tcpdump-2.png)

Server `157.245.128.27` feels like a [[Definitions and Topics/C2\|C2]] server that host `192.168.99.51` is beaconing to.
1. The target server is flagged as malicious on VirusTotal.com
2. The target server appears to be masquerading as an NTP/time synchronization server
3. The host machine is checking in with the target server about every minute over port 443
4. The host machine is checking in with a different NTP server, and the traffic looks very different.

And we're correct! Here's the closing paragraph below:
![BHIS-SOCC-lab-tcpdump-4.png](/img/user/Attachments/BHIS-SOCC-lab-tcpdump-4.png)

With [[Tool Deep-Dives/tcpdump\|tcpdump]], we were unable to see the details on the certificate that AC found, but without any idea of what was wrong, we were able to identify the malicious activity and flag the host as compromised.