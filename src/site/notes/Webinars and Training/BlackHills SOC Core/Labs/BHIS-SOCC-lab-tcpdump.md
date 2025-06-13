---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/labs/bhis-socc-lab-tcpdump/"}
---



 A quick reminder on tcpdump before we get started:


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/tcpdump/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">




#### tcpdump
*tcpdump* is a lightweight, cross-platform^[called windump on Windows] packet capture tool. It can be fully automated to capture audit trail of all traffic

- `tcpdump -D`
	- Lists interfaces
- `tcpdump -XA`
	- X is for Hex
	- A is for ASCII
- `tcpdump -w`
	- w is to write the data to a file





# Metadata

### Sources
[Home \| TCPDUMP & LIBPCAP](https://www.tcpdump.org/)
[WinDump - Home](https://www.winpcap.org/windump/)
### Tags
#tools_

</div></div>


## The tcpdump Lab
 [TCPdump Labs](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/TCPDump/TCPDump.md)


So first we get into the Linux terminal and `cd` to the folder with the PCAP (which is the root of the ADHD user profile), open it, and sort by the host machine.
- `tcpdump -n -r magnitude_1hr.pcap host 192.168.99.52`
	- `-n` 
		- Instructs tcpdump not to resolve the hostnames
		- This is key, because otherwise it stops every couple seconds to resolve domain names
	- `-r <file>` 
		- Identifies the file to modify
		- **NOTE**: It is helpful, nay necessary, to spell the filename correctly.
			- You can enter the first letter of the file and then press *tab* to autofill the file name

Many of the entries in the PCAP are broken up roughly like this:

```XML
<time> <protocol> sourceip.protocol>targetip.port: flags seq# ack# <window size> <packet length> <packet contents protocol>
```

Anyway, lots here to unpack, but also a lot of chaff to sort through, so we'll filter further. Tack on `and port 80` to filter only traffic through port 80 for HTTP traffic (or any traffic going over port 80).

You can also string ports together using `or`, for example, `tcpdump -n -r magnitude_1hr.pcap host 192.168.99.52 and port 443 or 80`.

We were already seeing a bunch of HTTP packets, so to help get a clearer picture of what's going on, we can add `-A` to show the packet contents, and see what data is being transmitted. The output blows past, but we can pipe the output to `less` to make it easier to scroll.

`tcpdump -n -r magnitude_1hr.pcap host 192.168.99.52 and port 443 or 80 -A | less`

In the 4th packet, we see that the "Referer" and "Host" addresses are highly unusual (Botswana Bank and a costume site), and in 8th packet, we see that PowerShell is being sent from an external IP to our host private IP address, and it's encoded in [[Tool Deep-Dives/Base64\|Base64]] to obfuscate the malicious code. Bad-bad!

![BHIS-SOCC-lab-tcpdump.png](/img/user/Attachments/BHIS-SOCC-lab-tcpdump.png)

> You can also filter by protocol; in the lab, they append `ip6` to the command to show only IPv6 entries, but you could filter by `arp`, `stp`, but not `lldp` for some reason.

# Bonus Lab 1
[Malware of the Day - What Time Is It? - Active Countermeasures](https://www.activecountermeasures.com/malware-of-the-day-what-time-is-it/)
Now that we have the basics, let's take a look at the most recent^[At the time of writing, 2024/02/19] Malware of the Day (linked at the bottom of the tcpdump lab).

I download the 24-hour PCAP file and navigate to it using the Ubuntu shell.

```Shell
cd /mnt/c/users/adhd/Downloads
tcpdump -n -r timeservsync_24r.pcap
```

So this PCAP comes from a networking device (and not a host), which we can tell because it lists traffic between an various public IPs and various Private (i.e., 192.168.0.0/16) addresses.

I haven't looked at the preamble for this malware, so let's start with the OWASP Top 10 ports (ignoring HTTP/S for now)


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/webinars-and-training/black-hills-soc-core/topics/socc-01-networking-and-pca-ps/#owasp-top-10-ports" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



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


# Bonus Lab 2
[Malware of the Day - XenoRAT - Active Countermeasures](https://www.activecountermeasures.com/malware-of-the-day-xenorat/)

It's been a while since I've done packet analysis, and I haven't read the blogpost talking about this, but let's see if we can figure out what's going on.

Like in Bonus Lab 1, I started with searching the PCAP file for all ports but HTTP/S.

`tcpdump -n -r xenorat_24hr.pcap port 23 or 22 or 3389 or 445 or 139 or 21 or 135 or 25 | less`

![BHIS-SOCC-lab-tcpdump-5.png](/img/user/Attachments/BHIS-SOCC-lab-tcpdump-5.png)

I'm seeing a lot of traffic between 192.168.2.14, .19, .22, and .77. They're all local, and after a period of time, it just becomes chatter between .14 and .77. Any of these could be the infected machine, but let's search for 192.168.2.77. Why .77? Because if either of .14 or .77 is a server, it's probably the one lower in the IP range since Admins are likely to reserve the first part of the IP range for static IPs.

> Because there's a lot of chatter between .14 and .77, I'm going to exclude .14 from the results.
> Also, if do the reverse (exclude .77 results), we that it doesn't really talk with any external machines.

`tcpdump -n -r xenorat_24hr.pcap host 192.168.2.77 and not 192.168.2.14 | less`

![BHIS-SOCC-lab-tcpdump-6.png](/img/user/Attachments/BHIS-SOCC-lab-tcpdump-6.png)

Well, look at that; notice a pattern? Regular check-ins with IP 172...75 carrying no data. It's also communicating on port 4444, which is a bit suspicious as well.

[Port 4444 (tcp/udp) :: SpeedGuide](https://www.speedguide.net/port.php?port=4444)

So I would bet that the host at IP 172...75 is probably a C2 server of some kind, and .77 is probably infected and should be investigated further.

And from the blog post, it looks like we're correct! 

![BHIS-SOCC-lab-tcpdump-7.png](/img/user/Attachments/BHIS-SOCC-lab-tcpdump-7.png)

Of course we don't have access to other system logs and processes to see how the attack was done, but it's a good place to start.