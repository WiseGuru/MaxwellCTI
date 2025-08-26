---
{"dg-publish":true,"permalink":"/definitions-and-topics/spf/","updated":"2025-08-01T12:20:27.814-07:00"}
---

#### SPF
- *SPF* (Sender Policy Framework) is a mechanism that identifies email servers that are authorized to send email from your domain.
	- Uses a TXT record^[There used to be an SPF record type, but that is no longer in use.] on your DNS host/nameserver
	- Recipients perform a DNS lookup to confirm the sender.
	- If there is no [[Definitions and Topics/DMARC\|DMARC]] record present (or the receiving server is not configured to check DMARC), then mail will be authorized or denied based on the SPF record policy.
- Receivers perform an *SPF Lookup* by comparing the incoming mail's sending server to the list of authenticated senders in the SPF record.^[More technically, the receiving server fetches the SPF TXT record from the sending domain in the SMTP `MAILFROM` field, and checks whether the sending IP address is listed in the record.]
	- If the email comes from an authorized sender, the email will `pass` the SPF lookup.
		-  [[Definitions and Topics/DMARC\|DMARC]] would check for *SPF Alignment* by comparing the SMTP `MAILFROM` and the email's "From:" fields to make sure they match.
	- If the sender is not authenticated, then the email either `fail` or `softfail` the SPF lookup.
		- This means the email will rely on [[Definitions and Topics/DKIM\|DKIM]] to pass [[Definitions and Topics/DMARC\|DMARC]] and could result in undelivered mail.
			- SPF alignment often fails in cases where email is automatically forwarded (e.g., from a professional account to a personal account, or if the email is sent to a distribution list), so it is best to have both *SPF* and [[Definitions and Topics/DKIM\|DKIM]] enabled where possible.
	- If the SPF record is missing, most receivers will treat is as a `none` or `neutral` result.
		- This means that the receiver's policies will play a bigger role in whether the email is delivered or not.
- Each sending mail server/domain must be identified
	- This can be directly via IP or through a domain/DNS lookup
		- SPF is limited to *10 DNS lookups*, and is evaluated all at once.
			- Going over 10 causes a `PermError`.
		- `TempError` is often caused by a transient (e.g. temporary) DNS lookup error.
			- It can be caused by recipient policies or a DNS lookup timeout; there may not be much you can do, but you should still check your record to make sure it's well below 10 look ups.^[dmarcian's [SPF Surveyor](https://dmarcian.com/spf-survey) is a great tool that identifies the number of lookups at each step.]
		- Once the SPF record is validated, the receiving email server matches the sending email server's address against the SPF record, stopping at the first match and corresponding action (pass, softfail, etc.)
- Use subdomains for third-party services where possible.
	- Configuring a unique subdomain (like `newsletter.example.com`) for email marketing services can help you stay below the 10 lookup limit with a separate SPF record isolates reputation harm from from misconfigured vendors.
	- *It is not recommended to add marketing services, like Mailchimp or Sendgrid, to your main SPF record*
		- Marketers use tons of servers to send mail to get around spam filters, and because of this, SPF DNS lookups often reach their limit before getting to the root IP and fail authentication.
- SPF [[Definitions and Topics/AAA\|Authorizes]] a list of approved senders and provides weak^[Since it does not cryptographically verify content like [[Definitions and Topics/DKIM\|DKIM]]] [[Definitions and Topics/AAA\|Authentication]] by comparing the sending IP against the list of approved senders.

#### SPF Implementation
[The syntax guide on Open-SPF is phenomenal](http://www.open-spf.org/SPF_Record_Syntax/), but for simplicity, here's an example of a sub-optimal SPF record. 

> [!Tip] SPF Qualifiers
> SPF uses qualifiers to indicate whether a source is permitted or unauthorized. They're also used to enforce policy if there's no accompanying [[Definitions and Topics/DMARC\|DMARC]] record or if the receiving server is not equipped to handle DMARC policies.
> 
> 1. `+` = *Pass*, the email originating host is allowed to send.
> 	1. This is the default, and does not need to be explicitly written out.
> 2. `-` = *Fail*, the originating host is not allowed to send and messages should be rejected.
> 3. `~` = *SoftFail*, originating host is not officially allowed to send, but authentic mail may be sent from it.
> 4. `?` = *Neutral*, or no policy.

1. **Name**: `@`
2. **Type**: TXT
3. **TTL**: 3600
4. **Value**: `v=spf1 +a mx ip4:123.45.67.89 a:contoso.com include:spf.example.com ~include:olddomain.com -all`

The SPF record is first checked as a whole to ensure it is a valid record and falls below the 10 maximum DNS lookups. Then the record is processed in order, and once the email is matched to a qualifier, it stops getting processed. Let's break it down.

1. **Name**: `@`
	1. `@`
		1. Designates the domain to which the SPF record applies; `@` is the current top-level domain (e.g., `example.com`)
		2. Can also use a subdomain, like `mail` or `mail.example.com`, with the exact formatting dependent on your DNS name server.
2. **Type**: TXT
	1. This is a text record (as opposed to A, CNAME, etc.)
3. **TTL**: 3600
	1. The *time to live* is 1 hour (3600 seconds)
	2. This is an expiration date for the SPF record, and helps DNS servers maintain up-to-date records.
4. **Value**: `v=spf1 +a mx ip4:123.45.67.89 a:contoso.com include:spf.example.com -all`
	1. `v=spf1`
		1. SPF version 1
		2. There is currently only one version.
	2. `+a`
		1. The `+` qualifier passes emails which match this sender
			1. Because the `+` is the default qualifier, is not normally explicitly stated, and will not be stated in further records.
		2. The `a` tells the recipient to check the current domain's DNS for A records (e.g., IPv4 addresses) and mark them as designated senders
		3. **Note**: dmarcian recommends avoiding `a` entries as they are often unnecessary and increase the number of lookups.^[[SPF Best Practices: Avoiding SPF Record Flattening - dmarcian](https://dmarcian.com/spf-best-practices/)]
	3. `mx`
		1. Check the current domain's DNS for MX records and mark them as designated senders
			1. **Note**: Since the default mechanism is `+`, it does not need to be explicitly written out
		2. The `mx` mechanism resolve to the A/AAAA records, which can trigger additional lookups.
			1. **Note**: dmarcian recommends avoiding `mx` entries as they are often unnecessary and increase the number of lookups.^[[SPF Best Practices: Avoiding SPF Record Flattening - dmarcian](https://dmarcian.com/spf-best-practices/)]
	4. `ip4:123.45.67.89`
		1. Designates the IP address `123.45.67.89` as an authorized sender
		2. Could also be used to designate an IP range (e.g., `ip4:123.45.67.0/24`)
		3. **Note**: Use `ip6:` to designate an IPv6 address or range.
	5. `a:contoso.com`
		1. Checks `contoso.com` for A and AAAA records^[Remember that A records are IPv4 addresses and AAAA records are for IPv6 addresses.] and designates them as authentic senders
	6. `include:spf.example.com`
		1. Checks `spf.example.com` for its SPF record and includes that in the current SPF record lookup
		2. Be careful as this can bring you much closer to the *10 DNS lookup limit* and cause a `PermError`
	7. `~include:olddomain.com`
		1. This marks emails from `olddomain.com` with a "softfail" qualifier, indicating that emails from that domain shouldn't be rejected outright, but possibly tagged as suspicious.
	8. `-all`
		1. Fail all originating hosts that have not been matched to this point in the record
			1. Because this is the last entry, any email that is authenticated by one of the earlier entries will get through, and everything else will fail authentication
			2. This is similar to Firewall configurations where the last rule is often `deny any any`
		2. Because this is a *Fail* qualifier, it must to be manually written out as `-`
			1. `~all` is also frequently seen in default configurations, and is used when transitioning between services or when using email marketing services (Mailchimp, Sendgrid, etc.) which do not get added to the SPF record.
		3. The last entry is always an `all` mechanism, which sets the qualifier for any email that doesn't match any earlier mechanism.

##### Honorable mention: `redirect`
 If you manage multiple domains and subdomains that all point reference the same record and information, you can configure them to all point to the same SPF record with a `redirect`. This replaces the entire SPF record and does not count towards the DNS lookup limit, and can reduce the headache of managing multiple identical SPF records.

A subdomain that redirects to an "spf-primary" record would look like this: `mailer.example.com    txt    "v=spf1 redirect=spf-primary.example.com"`

#### [[Tool Deep-Dives/dig\|dig]]

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/dig/#using-dig-to-verify-spf-dkim-and-dmarc" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Using [[Tool Deep-Dives/dig\|dig]] to verify [[Definitions and Topics/SPF\|SPF]], [[Definitions and Topics/DKIM\|DKIM]], and [[Definitions and Topics/DMARC\|DMARC]]
When using dig to verify records and look up changes, only SPF can be searched generally; DKIM and DMARC searches must include the full name of the record.

In the examples, I rotated the DNS server I was querying to demonstrate that any DNS server could be used. Records for subdomains or named records above root (e.g., `mail.example.com` or `_dmarc.example.com`) will require their own dig queries.

1. SPF
	1. `dig @8.8.8.8 example.com TXT +noall +answer`
		1. This will return all root TXT records, so you may get several irrelevant responses in addition to the SPF
2. DKIM
	1. `dig @1.1.1.1 mx01._domainkey.example.com TXT +noall +answer`
		1. Returns the final DKIM record value, whether stored on your name server or redirected via a CNAME
	2. `dig @1.0.0.1 protonmail._domainkey.example.com CNAME +noall +answer`
		1. Only returns the CNAME record on your server, if applicable.
3. DMARC
	1. `dig @8.8.4.4 _dmarc.example.com TXT +noall +answer`
		1. This will return the DMARC record for `example.com`
	2. `dig @1.1.1.1 _dmarc.mail.example.com txt +noall +answer`
		1. Returns the DMARC record for the subdomain `mail.example.com`

If the record exists and you entered the command correctly, you should get responses with all the information in the record being queried.

</div></div>

#### Managing DNS Lookups and DNS Flattening
Going above 10 DNS lookups will cause a permanent error for your SPF record, preventing it from being used for authentication. Because most SPF records will point to external mail services whose structure could change without notice, it's important to stay as far below 10 as possible so you don't accidentally begin erroring out.

There are a few tools you can use to check the number of lookups you have:
- [SPF Surveyor - dmarcian](https://dmarcian.com/spf-survey)
	- Well organized and easy to read.
- [EasyDMARC SPF Checker](https://easydmarc.com/tools/spf-lookup)
	- Similar to dmarcian, though a little less clean.
- [SPF Record Lookup - DMARCLY](https://dmarcly.com/tools/spf-record-checker)
	- Pretty spartan, but gets the job done.
	- Recommends *flattening*, which is bad practice.

##### What can you do to reduce lookups?
1. Remove unnecessary and duplicate records
	1. `a` and `mx` records are often unnecessary:
		1. `a` records identify domain-name lookup addresses, like `store.example.com`, and modernly the same IPs are not often reused for mail (especially with third-party email providers).
		2. `mx` records instruct sending domains where you *receive* email, and while intuitively it's the same place as where you *send* email from, this is often not the case (especially with third-party email providers).
	2. Remove any old or unused vendors or mechanisms
2. Consolidate where possible
	1. If there are overlapping SPF records for different services you use, you may be able to eliminate one of them. The tools listed above can help you find all sub-includes and IP ranges to determine eligibility.
3. Remove records from mass-marketing vendors
	1. Mass-marketing vendors (like MailChimp or SendGrid) do not work well with SPF, and will dramatically inflate the number of lookups you will perform. It's better to just configure [[Definitions and Topics/DKIM\|DKIM]] for them instead.
4. Use subdomains to split up certain senders
	1. If certain groups or departments use automated systems to send email, you could create a subdomain for that group or purpose that isolates its records from the main domain.
		1. e.g., if you use Acme Invoices as your billing platform, you could create `billing.example.com` and configure it with just Acme Invoice's SPF records to remove it from `example.com`'s SPF record.
5. Configure [[Definitions and Topics/DKIM\|DKIM]]
	1. Do the best you can, then configure [[Definitions and Topics/DKIM\|DKIM]]; you technically only need either DKIM or SPF to pass [[Definitions and Topics/DMARC\|DMARC]], and configuring both can provide coverage if one or the other fails.

##### What is flattening, and why is it bad?
Flattening an SPF record is the process of condensing all DNS lookups (e.g., `include:spf.example.com`, `a:contoso.com`, etc.) into hard-coded IP addresses. This gets around the lookup problem by not requiring any lookups, and if you control the servers that send email, this could be just fine. However, if you rely on third-parties for your email needs, flattening will get you into trouble.

Third-party services may add, delete, or change their infrastructure without informing you. Using hard-coded IPs will open you up to bad-actors who take over the abandoned IPs, or cause legitimate email to fail authentication because the vendor added new IPs that are not part of your hard-coded SPF record. Unless you manage every part of your email infrastructure, it's best to avoid flattening.



# Metadata

### Sources
[SPF: Project Overview](http://www.open-spf.org/)
[SPF: SPF Record Syntax](http://www.open-spf.org/SPF_Record_Syntax/)
[Why SPF Authentication Fails: none, neutral, fail(hard fail), soft fail, temperror, and permerror Explained - DMARCLY](https://dmarcly.com/blog/why-spf-authentication-fails-none-neutral-fail-hard-fail-soft-fail-temperror-and-permerror-explained)
### Tags
#defs_sec 