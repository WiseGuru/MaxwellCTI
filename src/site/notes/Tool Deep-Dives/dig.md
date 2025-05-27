---
{"dg-publish":true,"permalink":"/tool-deep-dives/dig/","noteIcon":""}
---

#### dig
- `dig` "is a network administration command-line tool for querying the [[Definitions and Topics/DNS\|Domain Name System (DNS)]]."^[[dig (command) - Wikipedia](https://en.wikipedia.org/wiki/Dig_(command))]
- dig follows a simple command structure, with various additional options
	- `dig @server name type`
		- `@server`: The DNS server being queried (referring to the system's DNS server if none are identified)
			- I recommend querying specific DNS servers, as I've had trouble relying on local DNS servers
		- `name`: The name of the resource (e.g. domain) to be looked up
		- `type`: The type of records to be retrieved (e.g., A, MX, NS, etc.), by default A records
			- You can use the `ANY` record type, but it's sometimes blocked as spam or gives incomplete results; see below for reference.
	- Common Options
		- `+short`: provides just a short answer to the query
		- `+noall +answer`: removes all information in the response and includes only the answer

#### Using [[Tool Deep-Dives/dig\|dig]] to verify [[Definitions and Topics/SPF\|SPF]], [[Definitions and Topics/DKIM\|DKIM]], and [[Definitions and Topics/DMARC\|DMARC]]
When using dig to verify records and look up changes, only SPF can be searched generally; DKIM and DMARC searches must include the full name of the record. In the examples, I rotated the DNS server I was querying, but just as an example.

1. SPF
	1. `dig @8.8.8.8 example.com TXT +noall +answer`
	2. *NOTE*: This will return all root TXT records, so you may get several responses
2. DKIM
	1. `dig @1.1.1.1 default._domainkey.example.com TXT +noall +answer`
3. DMARC
	1. `dig @8.8.4.4 _dmarc.example.com TXT +noall +answer`

If the record exists and you entered the command correctly, you should get responses with all the information in the record being queried.
#### Issues with ANY types
One of the issues with the `dig ANY` command is that the results can be wildly incomplete; this is because the responses can be pretty large and resource-intensive to generate, and can be used in [[DoS\|DoS]] attacks. 

Below are the results of the same query against three different DNS servers; notice how the results are very different between servers. 

![dig.png](/img/user/Attachments/dig.png)

However, if I query [[Definitions and Topics/SOA\|SOA]] type records, I get the same information, which was missing from two of the three ANY queries.

![dig-1.png](/img/user/Attachments/dig-1.png)

Using the `+noall +answer` query options appear to return more complete information, but still inconsistent.

![dig-2.png](/img/user/Attachments/dig-2.png)


# Metadata

### Sources
[dig (command) - Wikipedia](https://en.wikipedia.org/wiki/Dig_(command))
[Manual Pages — BIND 9 9.19.24 documentation](https://downloads.isc.org/isc/bind9/cur/9.19/doc/arm/html/manpages.html#dig-dns-lookup-utility)
### Tags
#tools_sec 