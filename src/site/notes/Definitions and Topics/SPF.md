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
		- SPF is limited to *10 DNS lookups*, and is evaluated all at once.
			- Going over 10 causes a `PermError`.
		- `TempError` is often caused by a transient (e.g. temporary) DNS lookup error.
			- It can be caused by recipient policies or a DNS lookup timeout; there may not be much you can do, but you should still check your record to make sure it's well below 10 look ups.^[dmarcian's [SPF Surveyor](https://dmarcian.com/spf-survey) is a great tool that identifies the number of lookups at each step.]
		- Once the SPF record is validated, the receiving email server matches the sending email server's address against the SPF record, stopping at the first match and corresponding action (pass, softfail, etc.)
	- It is not recommended to add marketing services, like Mailchimp or Sendgrid, to your main SPF record
		- Marketers use tons of servers to send mail to get around spam filters, and because of this, SPF DNS lookups often reach their limit before getting to the root IP and fail authentication.
		- Configuring a unique subdomain (like `newsletter.example.com`) for email marketing services allows you to create an SPF record just for that subdomain and isolates email reputation damage.
- [[Definitions and Topics/AAA\|Authorizes]] a list of approved senders and provides weak [[Definitions and Topics/AAA\|Authentication]] by comparing the sending IP against the list of approved senders.

#### SPF Implementation
Honestly, [the syntax guide on Open-SPF is phenomenal](http://www.open-spf.org/SPF_Record_Syntax/), but here's a breakdown of a fictional and not necessarily optimal record for quick reference. 

The SPF record is processed in order, but the whole record needs to be fully evaluated for the lookup to complete without a `permerror`.

1. **Name**: `@`
	1. `@`
		1. Designates the domain to which the SPF record applies; `@` is the current top-level domain (e.g., `example.com`)
		2. Can also use a subdomain, like `mail` or `mail.example.com`, with the exact formatting dependent on your DNS name server.
2. **Value**: `v=spf1 +a mx ip4:123.45.67.89 a:contoso.com include:mail.example.com -all`
	1. `v=spf1`
		1. SPF version 1
		2. There is currently only one version.
	2. `+a`
		1. The `+` is a qualifier that says what to do if a sending domain matches that value; here are a few of the most common ones.
			1. `+` = *Pass*, the email originating host is allowed to send
				1. This is the default, and does not need to be explicitly written out and is only here for demonstration purposes
			2. `-` = *Fail*, the originating host is not allowed to send
			3. `~` = *SoftFail*, originating host is not officially allowed to send, but authentic mail may be sent from it
		2. The `a` tells the recipient to check the current domain's DNS for A records (e.g., IPv4 addresses) and mark them as designated senders
			1. You might also see `aaaa` to designate an IPv6 address
		3. **Note**: dmarcian recommends avoiding `a` entries as they are often unnecessary and increase the number of lookups.^[[SPF Best Practices: Avoiding SPF Record Flattening - dmarcian](https://dmarcian.com/spf-best-practices/)]
	3. `mx`
		1. Check the current domain's DNS for MX records and mark them as designated senders
			1. **Note**: Since the default mechanism is `+`, it does not need to be explicitly written out
		2. **Note**: dmarcian recommends avoiding `mx` entries as they are often unnecessary and increase the number of lookups.^[[SPF Best Practices: Avoiding SPF Record Flattening - dmarcian](https://dmarcian.com/spf-best-practices/)]
	4. `ip4:123.45.67.89`
		1. Designates the IP address `123.45.67.89` as an authorized sender
	5. `a:contoso.com`
		1. Checks `contoso.com` for A records^[Remember that A records are IPv4 addresses.] and designates them as authentic senders
	6. `include:spf.example.com`
		1. Checks `spf.example.com` for its SPF record and includes that in the current SPF record lookup
		2. Be careful as this can bring you much closer to the *10 DNS lookup limit* and cause a `PermError`
	7. `-all`
		1. Fail all originating hosts that have not been matched to this point in the record
			1. Because this is the last entry, any email that is authenticated by one of the earlier entries will get through, and everything else will fail authentication
			2. This is similar to Firewall configurations where the last rule is often `deny any any`
		2. Because this is a *Fail* qualifier, it has to be manually written out as `-`
			1. `~all` is also frequently seen in default configurations, and is used when transitioning between services or when using email marketing services (Mailchimp, Sendgrid, etc.) which do not get added to the SPF record.

> Honorable mention: `redirect`
> If you manage multiple domains and subdomains that all point reference the same record and information, you can configure them to all point to the same SPF record with a `redirect`. It does not count towards the DNS lookup limit, and reduces the headache of managing multiple identical SPF records.
>  A subdomain that redirects to an "spf-primary" record would look like this: `mailer.example.com    txt    "v=spf1 redirect=spf-primary.example.com"`
#### Managing DNS Lookups and DNS Flattening
Going above 10 will cause a permanent error for your SPF record, preventing it from being used for authentication. Because most SPF records will point to external mail services whose structure could increase without notice, it's important to stay as far below 10 as possible so you don't accidentally begin erroring out.

There are a few tools you can use to check the number of lookups you have:
- [SPF Surveyor - dmarcian](https://dmarcian.com/spf-survey)
	- Well organized and easy to read.
- [EasyDMARC SPF Checker](https://easydmarc.com/tools/spf-lookup)
	- Similar to dmarcian, though a little less clean.
- [SPF Record Lookup - DMARCLY](https://dmarcly.com/tools/spf-record-checker)
	- Pretty spartan, but gets the job done.
	- Recommends flattening, which is bad practice.

##### What can you do to reduce lookups?
1. Remove unnecessary and duplicate records
	1. `a` and `mx` records are often unnecessary:
		1. `a` records identify domain-name lookup addresses, like `store.example.com`, and modernly the same IPs are not often reused for mail (especially with third-party email providers).
		2. `mx` records instruct sending domains where you *receive* email, and while intuitively it's the same place as where you *send* email from, this is often no the case (especially with third-party email providers).
	2. Remove any old or unused vendors or mechanisms
2. Consolidate where possible
	1. If there are overlapping SPF records for different services you use, you may be able to eliminate one of them. The tools listed above can help you find all sub-includes and IP ranges to determine eligibility.
3. Remove records from mass-marketing vendors
	1. Mass-marketing vendors (like MailChimp or SendGrid) do not work well with SPF, and will dramatically inflate the number of lookups you will perform. It's better to just configure [[Definitions and Topics/DKIM\|DKIM]] for them instead.
4. Configure [[Definitions and Topics/DKIM\|DKIM]]
	1. Do the best you can, then configure [[Definitions and Topics/DKIM\|DKIM]]; you only need either DKIM or SPF to pass [[Definitions and Topics/DMARC\|DMARC]], and configuring both can provide coverage if one or the other fails.

##### Why not flattening?
Flattening an SPF record is the process of condensing all DNS lookups (e.g., `include:spf.example.com`) into hard-coded IP addresses. This gets around the lookup problem by not requiring any lookups, and if you control the servers that send email, this could be just fine. However, if you rely on third-parties for your email needs, flattening will get you into trouble.

Because those third-party services may add, delete, or change their infrastructure without informing you, those hard-coded IPs will open you up to bad-actors who take over abandoned IPs, or legitimate email will not pass authentication because the vendor added new IPs that are not part of your hard-coded SPF record.



# Metadata

### Sources
[SPF: Project Overview](http://www.open-spf.org/)
[SPF: SPF Record Syntax](http://www.open-spf.org/SPF_Record_Syntax/)
[Why SPF Authentication Fails: none, neutral, fail(hard fail), soft fail, temperror, and permerror Explained - DMARCLY](https://dmarcly.com/blog/why-spf-authentication-fails-none-neutral-fail-hard-fail-soft-fail-temperror-and-permerror-explained)
### Tags
#defs_sec 