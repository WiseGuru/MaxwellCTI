---
{"dg-publish":true,"permalink":"/technical-guides/securing-email/"}
---

Securing your email is a critical part of protecting yourself and your business from scams and losses. The [FBI IC3 2024 Annual Report](https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf) identifies Business Email Compromise and Phishing/Spoofing as one of the biggest problems by far, causing a combined *$2.8 billion* in losses in 2024 alone.

One of the best ways to protect yourself and those you interact with is to secure and authenticate emails you send. **Without** *tools like [[Definitions and Topics/SPF\|SPF]], [[Definitions and Topics/DKIM\|DKIM]], and [[Definitions and Topics/DMARC\|DMARC]]* to authenticate your your messages, *scammers can send messages that appear to come from you without having access to your account or server*. 

Unfortunately, it's not a one-size-fits-all solution; many people will add a DKIM record but forget DMARC, or have a DMARC record where `p=none`, and these misconfigurations leave you vulnerable to attackers.

I have you covered; below are [[Technical Guides/Securing Email#Definitions\|Definitions]], [[Technical Guides/Securing Email#Implementation\|Implementation Guides]], and [[Technical Guides/Securing Email#Tools and Resources\|Resources]] that will help you configure and protect your email and brand identity.

>[!tip] Don't send email from your domain?
>If you don't send email from your domain, you only need **two DNS records that reject all email sent from your domain**:
> SPF: `TXT   @   "v=spf1 -all"`
> DMARC: `TXT  _dmarc.example.com   "v=DMARC1; p=reject"`
 > (note that `example.com` should be your domain, as in `_dmarc.maxwellcti.com`)

# Definitions

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/spf/#spf" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



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


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dkim/#dkim" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DKIM
- *DKIM* (Domain Keys Identified Mail) is a mechanism that authenticates all messages sent from a domain using asymmetric key cryptography.
	- DKIM uses two keys; a private key used for signing, and a public key for verification
	- The administrator publishes the public key to the DNS record
	- The sending server signs all out-bound messages by using the private key to generate a hash from the message headers and body.
	- Recipients verify the signed email against the public key
- The DKIM key is shared via a *TXT record*
	- DKIM records are differentiated with key selectors, and each email server requires its own public/private key pair.
		- You may have multiple DKIM records with different key selectors.
		- Because authentication occurs by key pair, tools like [MxToolBox](https://mxtoolbox.com/dkim.aspx) often require the selector to test and validate the correct key.
	- Can also be configured as a CNAME record^[Canonical name, which functions as an alias and points to another address.] which points to the actual DKIM TXT record.
		- CNAMEs are often used by service providers to allow them to manage and regularly cycle keys without bothering clients.
	- When rotating keys, you’d publish `selector1._domainkey` and `selector2._domainkey` simultaneously, switch signing to the new key, then remove the old record after clients have refreshed.
- Only [[Definitions and Topics/AAA\|servers with access to the private key]] are able to generate a valid signature, and proves the message was sent from an [[Definitions and Topics/AAA\|authorized]] sender.^[So keep that private key safe, or an attacker could forge messages!]


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dmarc/#dmarc" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DMARC
- *DMARC* (Domain-based Message Authentication, Reporting, and Conformance) is a mechanism that tells a receiving email server what to do based on the [[Definitions and Topics/SPF\|SPF]] and [[Definitions and Topics/DKIM\|DKIM]] authentication checks.
	- DMARC checks whether the domain passes the SPF and DKIM authentication checks and that the email's "From" field is *aligned* with the SPF and DKIM authenticated domains.
		- DMARC only checks for alignment if the authentication passes first.
	- If an email's "From" domain is aligned with *either* the SPF or DKIM authenticated domains, then it can be delivered.
- DMARC Alignment requirements can be "Relaxed" or "Strict"
	- Relaxed means email from a matching root-level (or organization level) domain will align
		- e.g., `marketing.example.com` will align with `example.com`
	- Strict means that the domain in the email must exactly match the authenticated domain
		- e.g., `marketing.example.com` would fail to align with `example.com`
- Delivery *aggregate* and *failure* reports can be sent to designated email addresses for review
	- These reports are critical for troubleshooting and setup.
		- *failure/forensic* reports are not generated by many mailbox providers for [[Definitions and Topics/GDPR\|GDPR]]/privacy regulation compliance, so you may be stuck with *aggregate reports*
	- DMARC can also be configured in purely an *audit mode* without SPF and DKIM; no action is taken, but you get reports on who is sending emails on your domain's behalf and whether they succeed for fail authentication.
	- Reports can be analyzed using [[Tool Deep-Dives/Python/DMARC Report Analyzer\|DMARC Report Analyzer]], which can ingest and process manually downloaded reports or connect automatically to an email account to download reports.
		- If you do not have an easy way to process these reports or create an account to receive them, services like [DMARC Report](https://dmarcreport.com/pricing/)^[This review offers a few others: [Free DMARC Monitoring Tools: We Test Out 7 Options](https://www.emailtooltester.com/en/blog/free-dmarc-monitoring/)] offer free report monitoring for a single domain and up to 10,000 email reports per month.
		- Additionally, Cloudflare offers free [DMARC report management](https://developers.cloudflare.com/dmarc-management/enable/) for domains configured with their service.
- There will most likely only be one DMARC TXT record on your DNS host.
	- Like [[Definitions and Topics/SPF\|SPF]], it applies to all emails sent from your domain, and not to specific hosts like [[Definitions and Topics/DKIM\|DKIM]]
	- The `sp` tag can be used to apply a different policy action on subdomains
		- However, if you want more granularity (like different aggregate/failure report addresses), you can add another record for that subdomain.
- DMARC verifies [[Definitions and Topics/AAA\|authentication]] by requiring alignment with either [[Definitions and Topics/SPF\|SPF]] or [[Definitions and Topics/DKIM\|DKIM]], specifies a policy instructing receivers how to handle [[Definitions and Topics/AAA\|unauthorized]] senders, and generates XML reports sent to the domain owner for [[Definitions and Topics/AAA\|Accounting]].

> If you are reviewing old records, you might see a DKIM record with `v=DKIM1; o=~`. This is an outdated and unused spec; it can be deleted without issue.^[[What is this extra \_domainkey.? Should I kill it? : r/DMARC](https://www.reddit.com/r/DMARC/comments/1h7elj3/comment/m0kwi0l/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)] ^[[draft-allman-dkim-ssp-01](https://datatracker.ietf.org/doc/html/draft-allman-dkim-ssp-01/#section-5)]


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/bimi/#bimi" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### BIMI
- *BIMI* (Brand Indicators for Message Identification) allows you to visually brand email sent by authenticated servers by adding your logo to the profile icon in recipient email clients.
- Most major email providers (e.g., Gmail) *require a trademarked image,* verified with a PEM certificate.




</div></div>


<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



#### MTA-STS and TLS-RPT
- *MTA-STS* (Mail Transfer Agent Strict Transport Security) is a mechanism by which all email sent to and from your email server is sent over [[Definitions and Topics/TLS\|TLS]] connections.
- *TLS-RPT* (TLS Reporting) is a standard that allows reporting of any encryption issues, and is helpful in triaging MTA-STS issues.






</div></div>



# Implementation
Below are implementation and syntax guides for each type of record; but to have a smooth rollout I recommend following these steps.

> [!Note]
> Many experts recommend configuring SPF and DKIM records at least 48 hours before DMARC,^[[Recommended DMARC rollout - Google Workspace Admin Help](https://support.google.com/a/answer/10032473?sjid=10183544884627146580-NC)] ^[[cisco.com/c/dam/en/us/products/collateral/security/esa-spf-dkim-dmarc.pdf](https://www.cisco.com/c/dam/en/us/products/collateral/security/esa-spf-dkim-dmarc.pdf)] but I don't think this is efficient.
> 
> Even though all email sent during this period will show as "failed authentication," setting up DMARC in *audit mode* as the first step will provide you with more information sooner about who is sending email on your organization's behalf, which can help you correct your SPF and DKIM records more quickly. There is no risk in creating DMARC record immediately as long as you set `p=none` so no action is taken on emails that fail authentication.

## Implementation Plan
1. **Configure DMARC to generate delivery reports**
	1. Create two email accounts dedicated to DMARC aggregate reports and failure/forensic reports
		1. For example, `dmarc_aggregate@example.com` and `dmarc_failures@example.com`
	2. Add the following DMARC policy to each domain you are managing (adding the appropriate addresses you created earlier):
		1. `v=DMARC1; p=none; rua=mailto:[aggregate report email address]; ruf=mailto:[failure report email address]; fo=1`
			1. This policy *takes no action on emails which fail authentication* and requests that *reports are sent to your designated email addresses.*
		2. If DMARC reports are going to be sent across different domains,^[Like if you're an MSP supporting support for a client.] then make sure you create a TXT DMARC report record (described below) on the receiving domain saying which domains are allowed to send DMARC reports.
			1. For example, if you want `example.com` to send DMARC reports to `aggregate@contoso.com`, you would need to create a DMARC report record on `contoso.com`'s DNS name server.
		3. `fo=1` will generate DMARC forensic reports for *either SPF or DKIM failures*, which can be helpful when troubleshooting problems during setup.
			1. Obligatory reminder that most providers do not send forensic reports, so `fo` and `ruf` can be ignored if desired.
	3. *Note*: Some providers require SPF and DKIM records before you can create a DMARC record, even in audit mode.
2. **Configure one SPF record** for each domain that identifies non-marketing mail senders
	1. Most mail providers have a tool to generate SPF records, or will have one available for you to copy and paste; review it and make sure it has everything you need and is strict enough for your risk appetite.
		1. Some email servers don't handle DMARC records, so try to use `-all` on your primary domains to reduce the likelihood of spoofing.
	2. *Do not add email marketers like Mailchimp or Sendgrid to your SPF record*; this will cause DNS lookups to exceed 10 hops and cause authentication failures.
	3. If you use an email marketing service provider, create a subdomain and set the last tag as `~all` and not `-all` to reduce the chance authentic messages are rejected as spam.
		1. Using a subdomain, like `newsletter.example.com`, to send email newsletters or marketing material allows you to create a unique set of SPF records for that subdomain.
		2. Unique subdomains also preserves the domain reputation for your primary domain and makes it easier to identify and isolate delivery problems.
3. **Configure DKIM records** for every sending host
	1. Each sender (Microsoft Exchange, Gmail, Mailchimp, etc.) will have instructions on how to add DKIM records to your name server.
4. **Review DMARC reports**
	1. It can take 48 to 72 hours for reports to start coming in.
		1. This is because DMARC records usually check for updates every 24 hours, and for global propagation it can take double that time.
		2. If you are not seeing aggregate reports after 48 hours, check the record to make sure there aren't any typos. [[Technical Guides/Securing Email#Resources\|The resources below can really help.]]
	2. These reports are XML documents and are frequently zipped before being attached
		1. *Over the next couple of weeks to couple of months*,^[[Google](https://support.google.com/a/answer/10032473?sjid=10183544884627146580-NC) recommends waiting at least a week, and that may be fine.] review these reports to see how mail is being delivered.
			1. MxToolBox has an [online DMARC Report Analyzer](https://mxtoolbox.com/DmarcReportAnalyzer.aspx) which converts the reports into an (online only) spreadsheet.
			2. If you're comfortable running scripts, [DMARC-Report-Analyzer](https://github.com/QbDVision-Inc/DMARC-Report-Analyzer) is a Python-based analyzer which can download files directly from your report inboxes and format them into a spreadsheet.
	3. Check to make sure that all authentic mail is passing SPF and DKIM authentication, and adjust your SPF and DKIM records as necessary.
		1. This is also a great way to uncover shadow IT and rogue service providers.
5. **Update SPF and DKIM records as needed**
	1. Chances are you will have missed a service during your initial setup or there's a misconfiguration.
	2. There is also a chance you will see inauthentic or malicious mail getting delivered.
		1. The forensic/failure reports give you a lot of information about the email, including the sender, subject, and recipient, so you can reach out to the recipient if necessary.
6. **Enforce DMARC policy**
	1. Once you are confident that authentic email won't fail authentication, change the `p=` tag in DMARC to `quarantine` or `reject` to begin blocking unauthenticated mail.
		1. Quarantine tells receiving servers to send failed email to junk, which can be helpful if you want to ensure that all legitimate email be delivered, but spoofing email will also get delivered to junk, increasing the chances of a victim falling for an attack.
		2. As mentioned before, it will take up to 48 hours for the changes to fully propagate.
	2. You can also add a `pct=` tag to only apply the policy to a percentage of unauthenticated mail
		1. For example, `pct=30` would only apply the policy to 30% of messages who fail SPF and DKIM authentication checks.
	3. You can now remove the `fo=1` tag from the DMARC record because you should have a very good idea about how your mail is authenticated and you only care about if mail isn't being delivered.
	4. Here's an updated version of the DMARC record we created earlier after ensuring that all authentic email is being delivered.
		1. `v=DMARC1; p=reject; rua=mailto:[aggregate report email address]; ruf=mailto:[failure report email address]`



<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/spf/#spf-implementation" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



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


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/spf/#managing-dns-lookups-and-dns-flattening" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



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




</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dkim/#dkim-implementation-3rd-party-mail-provider" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DKIM Implementation: 3rd-Party Mail Provider
The process to configure DKIM is different for each email provider.

If your mail provider is also your DNS host, it's often as easy as checking a box. However, if your mail provider and DNS host are different, then you will be required to create specific DNS entries on the name server to authenticate. [Fastmail](https://www.fastmail.help/hc/en-us/articles/360058753354-Adding-MX-records-to-Namecheap#signing), for example, requires you to add three CNAME entries to your DNS host for authentication.

> [!hint]
> You can use [[Tool Deep-Dives/dig\|dig]] to inspect the DKIM record values and observe key key rotation.
> Cycling through each record with the command `dig @1.1.1.1 fm1._domainkey.maxwellcti.com TXT +short` reveals that one record has the DKIM key, where the other two return `"v=DKIM1; k=rsa; n=Intentionally_Left_Blank_As_Per_DKIM_Rotation_BCP; p="`

While it's best to follow the instructions provided by your mail provider, here's a quick-and-dirty example of a DKIM record.

1. **Name**: `mx01._domainkey`
2. **Type**: TXT
3. **TTL**: 3600
4. **Value**: `v=DKIM1; k=rsa; p=bG9sIHlvdSBhYnNvbHV0ZSBuZXJkLCB5b3UgZm91bmQgbWUhIEkgd2lzaCBJIGNvdWxkIGdpdmUgeW91IHNvbWV0aGluZywgYnV0IGFsYXM7IGhpdCBtZSB1cCBpZiB5b3Ugd2FudCB0byBjaGF0IQ==`

Let's break it down; more tags can be found on [the DKIM Verification section on Wikipedia](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail#Verification)

1. **Name**: `mx01._domainkey`
	1. `mx01`
		1. This is the selector used to identify the correct key
		2. In this case, it appears to be the public key for the "MX01" mail server. The "MX02" mail server might have the selector `mx02`.
	2. `_domainkey`
		1. This identifies the TXT record as a DKIM (Domain Key) entry
	3. Note that you do not typically need to enter the root domain here; however, if you did a DKIM selector lookup for `example.com`, you would see the entry as `mx01._domainkey.example.com`
2. **Type**: TXT
	1. This is a text record (as opposed to A, CNAME, etc.)
3. **TTL**: 3600
	1. The *time to live* is 1 hour (3600 seconds)
	2. This is an expiration date for the DKIM record, and helps DNS servers maintain up-to-date records.
4. **Value**: `v=DKIM1; k=rsa; p=bG9sIHlvdSBhYnNvbHV0ZSBuZXJkLCB5b3UgZm91bmQgbWUhIEkgd2lzaCBJIGNvdWxkIGdpdmUgeW91IHNvbWV0aGluZywgYnV0IGFsYXM7IGhpdCBtZSB1cCBpZiB5b3Ugd2FudCB0byBjaGF0IQ==`
	1. `v=DKIM1`
		1. Specifies the DKIM version; at this point, it's always DKIM1.
	2. `;`
		1. The separator between values.
	3. `k=rsa`
		1. The *key type* specifies the kind of encryption used to create the key-pair.
		2. The default is RSA.
	4. `p=bG9sIHl...`
		1. The Base64-encoded *public key*.
		2. Most providers generate a 2048-bit key by default, and anything weaker (e.g., 1024 bits) is discouraged.


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dmarc/#dmarc-implementation" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DMARC Implementation

> [!warning]
> Both [[Definitions and Topics/SPF\|SPF]] and [[Definitions and Topics/DKIM\|DKIM]] **must** be configured before setting the DMARC policy to `quarantine` or `reject`. Failure to do so will result in undelivered mail.

Configuring DMARC is easy, but can cause you the most headaches because it's what authorizes email to be delivered, and a misconfiguration can stop your email in its tracks. Therefore, it's highly recommended that you *first configure your DMARC policy to `none` to take no action* on emails for the first couple of weeks, using the reports generated to make sure everything is getting delivered as expected, and then to add a `quarantine` or `reject` policy and maybe ramp up implementation through the `pct` tag.

Below is an example of a DMARC TXT record:

1. **Name**: `_dmarc`
2. **Type**: TXT
3. **TTL**: 3600
4. **Value**: `v=DMARC1; p=quarantine; sp=reject; pct=100; aspf=r; adkim=r; rua=mailto:dmarc-reports@example.com; ruf=mailto:dmarc-failures@example.com; fo=1; ri=43200`

Let's break it down.

1. **Name**: `_dmarc.example.com`
	1. `_dmarc`
		1. Signifies this a DMARC TXT entry
	2. Note that you do not typically need to enter the root domain here; however, if you did a DMARC lookup for `example.com`, you would see the entry as `_dmarc.example.com`
		1. Only one DMARC record needs to exist for the apex domain, but you can add more records for different subdomains to take different actions; for example, `_dmarc.mailer.example.com`
2. **Type**: TXT
	1. This is a text record (as opposed to A, CNAME, etc.)
3. **TTL**: 3600
	1. The *time to live* is 1 hour (3600 seconds)
	2. This is an expiration date for the DMARC record, and helps DNS servers maintain up-to-date records.
4. **Value**: `v=DMARC1; p=quarantine; sp=reject; pct=100; aspf=r; adkim=r; rua=mailto:dmarc-reports@example.com; ruf=mailto:dmarc-failures@example.com; fo=1;`
	1. `v=DMARC1`
		1. DMARC version 1; at present, there is only one version.
	2. `;`
		1. The separator between tags.
		2. Spaces and tabs can make entries more readable, but are completely optional.
	3. `p=quarantine`
		1. The *Policy* applied to emails which fail their SPF and DKIM authentication checks
		2. There are three options:
			1. `none`: No action is taken
				1. Typically used while configuring email security and collecting reports.
			2. `quarantine`: Emails which fail authentication should be treated with suspicion and sent to the spam/junk folder
				1. This allows the recipient server to still receive and process unauthenticated mail, just treats them with suspicion
			3. `reject`: Instructs the receiving server to outright reject any mail that fails authentication.
				1. This is the most secure, but can also be the most problematic if a configuration changes or something goes wrong.
	4. `sp=reject`
		1. The policy for subdomains; if `sp` is not stated in the record, then the policy described by `p` applies to subdomains.
			1. This may be helpful to set a stricter policy if there are no subdomains, or a looser policy if configuring a subdomain for mail.
		2. Creating a distinct DMARC policy for a subdomain (e.g., `_dmarc.mailer.example.com`) takes precedence over the `sp` policy designation.
			1. To reiterate, `sp` only applies to a subdomain if there isn't a more specific DMARC policy created for it.
	5. `pct=100`
		1. The percent of unauthenticated emails to apply the policy to.
			1. e.g., `pct=20` would only apply the `p=reject` policy to 20% of emails which fail authentication
		2. This is helpful during a slow rollout to make sure not all email flow stops.
		3. Default is 100, and does not need to be explicitly written.
	6. `aspf=r`
		1. SPF alignment requirements
			1. `r` is relaxed, and only the root/organizational domain must match
				1. Relaxed is the default value for both `aspf` and `adkim`, and does not need to be explicitly stated
			2. `s` is strict, and domains must match exactly
	7. `adkim=r`
		1. DKIM alignment requirements; see `aspf` for details.
	8. `rua=mailto:dmarc-reports@example.com`
		1. Identifies the email address to which recipient servers should send *delivery aggregate reports*
		2. List of addresses must begin with `mailto:`
		3. Aggregate reports contain basic information and include successful and failed delivery information
		4. Multiple addresses can be specified if separated by a comma.
			1. e.g., `rua=mailto:address1@example.com,address2@contoso.com`
	9. `ruf=mailto:dmarc-failures@example.com`
		1. Identifies the email address to which recipient servers should send *individual delivery forensic failure reports*
		2. Forensic failure reports contain detailed information about failed deliveries to assist with triage and troubleshooting.
			1. However, most major providers highly redact or suppress ruf records for privacy and [[Definitions and Topics/GDPR\|GDPR]] compliance.
	10. `fo=1`
		1. The *failure reporting option* specifies what generates a forensic report .
			1. `0` is default, and only requests forensic reports on DMARC failure.
			2. `1` requests a report *for any SPF or DKIM failure*, which is helpful for triage and troubleshooting during initial setup.
			3. `d` generates a report only when DKIM authentication (not alignment)
			4. `s` generates a report when SPF fails.
		2. You can select multiple options with a colon (e.g., `fo=0:d`)
		3. *Reminder*: many mailbox providers don't send forensic/failure reports for [[Definitions and Topics/GDPR\|GDPR]] compliance.
	11. `ri=43200`
		1. The "report interval" is the time in seconds you request receivers to generate reports.
			1. The default is 24 hours (86400 seconds), and 12 hours as configured here.
		2. Most providers ignore this, and send reports every 24 hours or more based on their own infrastructure.
			1. "DMARC implementations MUST be able to provide daily reports and SHOULD be able to provide hourly reports when requested.  However, anything other than a daily report is understood to be accommodated on a best-effort basis."^[[RFC 7489 - Domain-based Message Authentication, Reporting, and Conformance (DMARC)](https://datatracker.ietf.org/doc/html/rfc7489#autoid-21)]
 
##### Sending Reports to a Different Domain
 If you are sending DMARC reports to another domain for analysis, you will need to create a TXT record on that domain's name server to identify each sending domain.^[[DMARC - DMARC External Validation](https://mxtoolbox.com/problem/dmarc/dmarc-external-validation?page=prob_dmarc&action=dmarc:annmulhern.com&showlogin=1&hidepitch=0&hidetoc=1)]
 
For example:
1. Type: `TXT`
2. Name: `sendingdomain.com._report._dmarc.receivingdomain.com`
	1. For example, if the MSP `AcmeIT.com` was configured to receive and manage DMARC reports for `example.com`, the record would be `example.com._report._dmarc.acmeit.com`
3. Value: `"v=DMARC1"`
	1. Just indicates the version of DMARC

The TXT record **name** identifies the domain generating the report (`sendingdomain.com`), followed by `._report._dmarc` and the domain receiving the reports (`.receivingdomain.com`).
The TXT record **value** just identifies the DMARC version (`"v=DMARC1"`)
 
**WARNING**: While you can use a `*` wildcard to simplify the record to `*._report._dmarc.receivingdomain.com`, allowing anyone to send email to your DMARC report inboxes, you probably shouldn't. Spam filters and firewalls don't typically inspect DMARC reports, and an attacker could exploit this to flood the inbox with bogus DMARC reports or inject malicious code into zipped attachments, which might get run automatically by report analyzing software. 


</div></div>


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


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/tool-deep-dives/python/dmarc-report-analyzer/#dmarc-report-analyzer" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DMARC Report Analyzer
- *DMARC Report Analyzer* is a tool written in [[Tool Deep-Dives/Python/Python\|Python]] that allows you to analyze all DMARC reports in a folder or inbox and generate a spreadsheet with the results.
- I've forked a version and added^[With the help of an LLM.] various features, including more progress bars, an offline cache for the Spamhaus list of IPs, and an output CSV which includes more information that can be helpful when troubleshooting [[Technical Guides/Securing Email\|email authentication issues.]]

To install and use [DMARC Report Analyzer](https://github.com/WiseGuru/DMARC-Report-Analyzer/tree/main), there is a [helpful script in the Installation section](https://github.com/WiseGuru/DMARC-Report-Analyzer?tab=readme-ov-file#installation) to clone the repository to your local machine, create the local environment, install the requirements, and then run `main.py`. First, however, I recommend creating a `scripts` (or similar) folder somewhere to keep everything organized and then opening your Terminal from that folder.

The script expects you to have Python version 3 installed on your system and is currently written for Linux, but there is an option for Windows (which I haven't really tested). 

If working on a [[Tool Deep-Dives/Linux/Linux\|Linux]] system, you will need to use a [[Tool Deep-Dives/Python/Python Virtual Environment\|Python Virtual Environment]] to download and update the correct packages. This is easily managed with a shell script that performs the run/setup process.

```bash
#!/usr/bin/env bash
cd ~/scripts/DMARC-Report-Analyzer
# Optional
python3 -m venv env
source env/bin/activate
# Mandatory
pip install -r requirements.txt
python3 main.py
```

The final result is a spreadsheet with a summary of all of the reports it collected. You can use [[Tool Deep-Dives/whois\|whois]] and [[Tool Deep-Dives/Linux/grep\|grep]] to lookup the IP addresses to to find out where emails are coming from with the command `whois [IP address] | grep "NetRange:\|CIDR:\|Organization:"`, and your output should look something like this:

![DMARC Report Analyzer.png](/img/user/Attachments/DMARC%20Report%20Analyzer.png)




</div></div>


# Tools and Resources
## Resources and tools
- [DKIM Test - DKIM Verify - DKIM Validator](https://www.appmaildev.com/en/dkim)
	- Straight-forward tool to test and analyze SPF, DKIM, and DMARC.
- [Learn and Test DMARC](https://www.dmarctester.com/)
	- Great site for quickly testing mail.
	- Note that the spoofing feature just walks you through what would happen if someone spoofed your domain, and does not generate an actual spoofed email.
- [MX Lookup Tool - Check your DNS MX Records online - MxToolbox](https://mxtoolbox.com/)
	- MxToolBox is one of the best online toolkits you can use; I have cultivated links here for each kind of check you might want to perform.
	- [Network Tools: DNS,IP,Email - MxToolBox](https://mxtoolbox.com/SuperTool.aspx)
	- [SPF Check & SPF Lookup - Sender Policy Framework (SPF) - MxToolBox](https://mxtoolbox.com/spf.aspx)
	- [DKIM Check- DomainKeys Identified Mail (DKIM) Record Lookup - MxToolBox](https://mxtoolbox.com/dkim.aspx)
	- [DMARC Check Tool - Domain Message Authentication Reporting & Conformance Lookup - MxToolBox](https://mxtoolbox.com/dmarc.aspx)
	- [BIMI Record Check - BIMI Lookup Tool - Brand Indicators for Message Identification Tools - MxToolBox](https://mxtoolbox.com/bimi.aspx)
	- [MTA-STS Lookup - Check domains for Inbound Transport Layer Security (TLS) Enforcement - MxToolbox](https://mxtoolbox.com/mta-sts.aspx)
- [DMARC Domain Checker - dmarcian](https://dmarcian.com/domain-checker/)
	- dmarcian offers another fabulous record checker with additional tools that break down SPF, DKIM, and DMARC records.
	- Be warned, however, that it only returns one DKIM key and has trouble finding CNAME records.
- [Free Domain Analyzer Tool \| PowerAnalyzer](https://powerdmarc.com/analyzer/)
	- Can automatically detect/select the DKIM selector
	- Checks BIMI and a few others with only a couple of clicks
- [[Tool Deep-Dives/Python/DMARC Report Analyzer\|DMARC Report Analyzer]]
	- Local, Python-based DMARC report analyzer
	- Can automatically log in to email accounts and download DMARC reports directly for analysis
- [Email authentication in Microsoft 365 - Microsoft Defender for Office 365 \| Microsoft Learn](https://learn.microsoft.com/en-us/defender-office-365/email-authentication-about)
	- Microsoft has a robust guide on configuring SPF, DKIM, and DMARC records for your organization.
- [Email Authentication Best Practices - Cisco](https://www.cisco.com/c/dam/en/us/products/collateral/security/esa-spf-dkim-dmarc.pdf)
	- I'm linking this document really just to rant; this "most optimal" "best practices" guide from Cisco is kind of crazy.
	- It may be helpful for larger environments, but their implementation strategy feels backwards to me.
		- It takes *6 and a half months* to get to the point where you're enacting policy, and you don't even setup a DMARC record until about the *third month*.
		- I think it's important, critical even, to be diligent when working with a production environment, and if you want to slow-roll deployment to make sure nothing goes wrong, all power to you, but to only start gathering live information half-way through deployment seems crazy to me.
- [GitHub - techsneeze/dmarcts-report-parser: A Perl based tool to parse DMARC reports from an IMAP mailbox or from the filesystem, and insert the information into a database. ( Formerly known as imap-dmarcts )](https://github.com/techsneeze/dmarcts-report-parser)

## DNS Look-up tools
[[Tool Deep-Dives/dig\|dig]]
[[nslookup\|nslookup]]
[[Tool Deep-Dives/whois\|whois]]