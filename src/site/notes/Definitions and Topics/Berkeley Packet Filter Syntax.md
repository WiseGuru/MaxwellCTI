---
{"dg-publish":true,"permalink":"/definitions-and-topics/berkeley-packet-filter-syntax/"}
---

### Berkeley Packet Filter Syntax (BPF)

1. The *expression* consists of one or more *primitives*
	1. **Primitives** usually consist of an *id* (name or number) preceded by one or more *qualifiers*^[[BPF syntax](https://biot.com/capstats/bpf.html)]
		1. **ID**s are specifically what you're searching for
			1. Could be a hostname, an IP address, subnet, etc.
		2. **Qualifiers** clarify and qualify what the *ID* is
			1. *type* identifies what kind of thing the ID is
				1. host, net, port, portrange
			2. *dir* specifies the *direction of traffic*
				1. src, dst, src and/or dst
				2. The default is *src or dst*
			3. *proto* identify the *protocol*
				1. ether, fddi, tr, wlan, ip, ip6, arp, rarp, decnet, tcp, udp



# Metadata

### Sources
[BPF syntax](https://biot.com/capstats/bpf.html)

### Tags
#defs_sec 