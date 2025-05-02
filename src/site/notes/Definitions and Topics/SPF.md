---
{"dg-publish":true,"permalink":"/definitions-and-topics/spf/","noteIcon":""}
---

#### SPF
- *SPF* (Sender Policy Framework) is a mechanism that identifies email servers that are authorized to send email from your domain.
	- Uses a TXT or SPF record on your DNS host/nameserver
	- Recipients perform a DNS lookup to confirm the sender.
	- If the SPF record is missing, or the sender is not authenticated, the message will fail and may not be delivered.
- Each sending mail server/domain must be identified
	- This can be directly via IP or through a domain/DNS lookup
		- SPF is limited to *10 DNS lookups*; going over 10 causes a `PermError`
	- It is not recommended to identify marketing services, like Mailchimp or Sendgrid, in SPF
		- Due to the high volume of email they send and the number of distinct email servers they use to get around spam restrictions, you can't identify an IP address and domain look-ups can timeout and cause problems for authentication
- Provides [[Definitions and Topics/AAA\|Authentication]] and [[Definitions and Topics/AAA\|Authorization]]

#### SPF Implementation
Honestly, [the syntax guide on Open-SPF is phenomenal](http://www.open-spf.org/SPF_Record_Syntax/), but here's a breakdown of a typical record^[Gussied up for use as an example.] for quick reference.
The SPF record is processed in order; as soon as an email is matched to one of the rules, it stops getting processed and passes or fails the check.

The name of the SPF record designates the domain its being applied to; it can be `@` or the current domain to reference the domain itself (e.g. `example.com`) or a subdomain like `mail.example.com`.

1. Name: `@`
	1. `@`
		1. Designates the domain to which the SPF record applies; `@` is the current top-level domain (e.g., `example.com`)
		2. Can also use a subdomain, like `mail` or `mail.example.com`, with formatting depending on your DNS nameserver.
2. Value: `v=spf1 +a mx ip4:123.45.67.89 a:contoso.com include:mail.example.com -all`
	1. `v=spf1`
		1. Version = SPF 1
	2. `+a`
		1. The `+` is a qualifier that says what to do if an email matches that value; here are a few of the most common ones.
			1. `+` = Pass, the email originating host is allowed to send; this is the default, and does not need to be explicitly written out and I only put it here for demonstration purposes
			2. `-` = Fail, the originating host is not allowed to send
			3. `~` = SoftFail, originating host is not allowed to send, but authentic mail may be sent from it
		2. The `a` tells the recipient to check the current domain's DNS for A records (e.g., IPv4 addresses) and mark them as designated senders
			1. You might also see `aaaa` to designate an IPv6 address
	3. `mx`
		1. Check the current domain's DNS for MX records and mark them as designated senders
			1. Note: Since the default mechanism is `+`, it does not need to be explicitly written out
	4. `ip4:123.45.67.89`
		1. Designates the IP address `123.45.67.89` as an authorized sender
	5. `a:contoso.com`
		1. Checks `contoso.com` for A records^[Remember that A records are IPv4 addresses.] and designates them as senders
	6. `include:mail.example.com`
		1. Checks `mail.example.com` for its own SPF record and includes that in the SPF record lookup
		2. Be careful as this can bring you much closer to the *10 DNS lookup limit* and cause a `PermError`
	7. `-all`
		1. Fail all originating hosts that have not been matched to this point in the record
			1. Because this is the last entry, any mail that is authenticated by one of the earlier entries will get through, and everything else is failed
			2. Similar to Firewall configuration where the last rule is often `deny any any`
		2. Because this is a *Fail* qualifier, it has to be manually written out as `-`
		3. `~all` is also frequently seen in default configurations
			1. Used when transitioning between services or when using email marketing services (Mailchimp, Sendgrid, etc.) which do not get added to the SPF record.



# Metadata

### Sources
[SPF: Project Overview](http://www.open-spf.org/)
[SPF: SPF Record Syntax](http://www.open-spf.org/SPF_Record_Syntax/)

### Tags
#defs_sec 