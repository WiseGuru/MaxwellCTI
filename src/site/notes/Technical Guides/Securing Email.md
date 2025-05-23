---
{"dg-publish":true,"permalink":"/technical-guides/securing-email/","noteIcon":""}
---

Securing your email is a critical part of protecting yourself and your business from scams and losses. The [FBI IC3 2024 Annual Report](https://www.ic3.gov/AnnualReport/Reports/2024_IC3Report.pdf) identifies Business Email Compromise and Phishing/Spoofing as one of the biggest problems by far, causing a combined *$2.8 billion* in losses in 2024 alone.

One of the best ways to protect yourself and those you interact with is to secure and authenticate emails you send. *Without tools like [[Definitions and Topics/SPF\|SPF]], [[Definitions and Topics/DKIM\|DKIM]], and [[Definitions and Topics/DMARC\|DMARC]]* to authenticate your your messages, *scammers can send messages that appear to come from you without having access to your account or server*. 

Great! Sign me up, right? While setup is be relatively easy, misconfigurations can cause major headaches and prevent mail from getting delivered, and old attempts can muddy the waters about what's good and bad. 

>*Don't send email from your domain*? Stop anyone from impersonating you with **these rules that reject all email sent from your domain**:
> SPF: `TXT   @   "v=spf1 -all"`
> DMARC: `TXT  _dmarc.example.com   "v=DMARC1; p=reject"`

Below are [[Technical Guides/Securing Email#Definitions\|Definitions]], [[Technical Guides/Securing Email#Implementation\|Implementation Guides]], and [[Technical Guides/Securing Email#Tools and Resources\|Resources]] that will help you configure and protect your email identity.

# Definitions

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/spf/#spf" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### SPF
- *SPF* (Sender Policy Framework) is a mechanism that identifies email servers that are authorized to send email from your domain.
	- Uses a TXT or SPF record on your DNS host/nameserver
	- Recipients perform a DNS lookup to confirm the sender.
	- If the SPF record is missing, or the sender is not authenticated, the message will fail and may not be delivered.
- Each sending mail server/domain must be identified
	- This can be directly via IP or through a domain/DNS lookup
		- SPF is limited to *10 DNS lookups*; going over 10 causes a `PermError`.
		- `TempError` is often caused by a transient (e.g. temporary) DNS lookup error.
			- It can be caused by recipient policies or a DNS lookup timeout; there may not be much you can do, but you should still check your record to make sure it's well below 10 look ups.^[dmarcian's [SPF Surveyor](https://dmarcian.com/spf-survey) is a great tool that identifies the number of lookups at each step.]
	- It is not recommended to add marketing services, like Mailchimp or Sendgrid, to your main SPF record
		- Marketers use tons of servers to send mail to get around spam filters, and because of this, SPF DNS lookups often reach their limit before getting to the root IP and fail authentication.
		- Configuring a unique subdomain (like `newsletter.example.com`) for email marketing services allows you to create an SPF record just for that subdomain and isolates email reputation damage.
- Provides [[Definitions and Topics/AAA\|Authentication]] and [[Definitions and Topics/AAA\|Authorization]]


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dkim/#dkim" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DKIM
- *DKIM* (Domain Keys Identified Mail) is a mechanism that authenticates all messages sent from a domain using [[Definitions and Topics/PKI\|PKI (Public Key Infrastructure)]]
	- DKIM uses two keys; a private key used for signing, and a public key for verification
	- The administrator publishes the public key to the DNS record
	- The sending server signs all out-bound messages using the private key
	- Recipients verify the signed email against the public key
- The DKIM key is shared via a *TXT record*
	- DKIM records are differentiated with key selectors, and each email server requires its own public/private key pair.
		- You may have multiple DKIM records with different key selectors.
		- Because authentication occurs by key pair, tools like [MxToolBox](https://mxtoolbox.com/dkim.aspx) often require the selector to test and validate the correct key.
	- Can also be configured as a CNAME record^[Canonical name, which functions as an alias and points to another address.] which points to the actual DKIM TXT record
- Provides [[Definitions and Topics/AAA\|Authentication]]


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dmarc/#dmarc" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DMARC
- *DMARC* (Domain-based Message Authentication, Reporting, and Conformance) is a mechanism that tells a receiving email server what to do based on the [[Definitions and Topics/SPF\|SPF]] and [[Definitions and Topics/DKIM\|DKIM]] authentication checks.
	- DMARC checks whether the domain in the email's "From" field matches (or is *aligned* with) the SPF or DKIM authenticated domains.
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
- There will most likely only be one DMARC TXT record on your DNS host.
	- Like [[Definitions and Topics/SPF\|SPF]], it applies to all emails sent from your domain, and not to specific hosts like [[Definitions and Topics/DKIM\|DKIM]]
	- The `sp` tag can be used to apply a different policy action on subdomains
		- However, if you want more granularity (like different aggregate/failure report addresses), you can add another record for that subdomain.

> If you are reviewing old records, you might see a DKIM record with `v=DKIM1; o=~`. This is an outdated and unused spec; it can be deleted without issue.^[[What is this extra \_domainkey.? Should I kill it? : r/DMARC](https://www.reddit.com/r/DMARC/comments/1h7elj3/comment/m0kwi0l/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)] ^[[draft-allman-dkim-ssp-01](https://datatracker.ietf.org/doc/html/draft-allman-dkim-ssp-01/#section-5)]


</div></div>


<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



#### BIMI
- *BIMI* (Brand Indicators for Message Identification) allows you to visually brand email sent by authenticated servers by adding your logo to the profile icon in recipient email clients.







</div></div>


<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



#### MTA-STS and TLS-RPT
- *MTA-STS* (Mail Transfer Agent Strict Transport Security) is a mechanism by which all email sent to and from your email server is sent over [[Definitions and Topics/TLS\|TLS]] connections.
- *TLS-RPT* (TLS Reporting) is a standard that allows reporting of any encryption issues, and is helpful in triaging MTA-STS issues.






</div></div>



# Implementation
Below are implementation and syntax guides for each type of record; but to have a smooth rollout I recommend following these steps.

> **Note**: Many experts recommend configuring SPF and DKIM records at least 48 hours before DMARC,^[[Recommended DMARC rollout - Google Workspace Admin Help](https://support.google.com/a/answer/10032473?sjid=10183544884627146580-NC)] ^[[cisco.com/c/dam/en/us/products/collateral/security/esa-spf-dkim-dmarc.pdf](https://www.cisco.com/c/dam/en/us/products/collateral/security/esa-spf-dkim-dmarc.pdf)] but I don't think this is efficient.
> 
> Even though all email sent during this period will show as "failed authentication," setting up DMARC in *audit mode* first will provide you with more information sooner about who is sending email on your organization's behalf, which can help you correct your SPF and DKIM records more quickly. There is no risk in setting up DMARC first as long as you set `p=none` so no action is taken on emails that fail authentication.

## Implementation Plan
1. **Configure DMARC to generate delivery reports**
	1. Create two email accounts; one for DMARC aggregate reports and one for failure/forensic reports
		1. For example, `dmarc_aggregate@example.com` and `dmarc_failures@example.com`
	2. Add the following DMARC policy to each domain you are managing (adding the appropriate addresses you created earlier):
		1. `v=DMARC1; p=none; rua=mailto:[aggregate report email address]; ruf=mailto:[failure report email address]; fo=1`
			1. This policy *takes no action on emails which fail authentication*, but *sends reports to your designated email addresses.*
		2. If DMARC reports are going to be sent across different domains,^[Like if you're an MSP supporting support for a client.] then make sure you create a TXT DMARC report record (described below) on the receiving domain saying which domains are allowed to send DMARC reports.
			1. For example, if you want `example.com` to send DMARC reports to `aggregate@contoso.com`, you would need to create a DMARC report record on `contoso.com`'s DNS name server.
		3. `fo=1` will generate DMARC forensic reports for *either SPF or DKIM failures*, which is very helpful when troubleshooting problems during setup.
2. **Configure one SPF record** for each domain that identifies non-marketing mail senders
	1. Most mail providers have a tool to generate SPF records, or will have one available for you to copy and paste; review it and make sure it has everything you need and is strict enough to your liking.
	2. *Do not add email marketers like Mailchimp or Sendgrid to your SPF record*; this will cause DNS lookups to exceed 10 hops and cause authentication failures.
		1. If you use an email marketing service provider, set the last tag as `~all` and not `-all` to make it less likely they get rejected as spam.
		2. Using a subdomain to send email marketing, like `newsletter.example.com`, allows you to create a unique set of SPF records for that subdomain, preventing cross-contamination of DNS lookups and tightening the authentication requirements for both personal and broadcast emails. 
3. **Configure DKIM records** for every sending host
	1. Each sender (Microsoft Exchange, Gmail, Mailchimp, etc.) will have instructions on how to add DKIM records to your name server.
4. **Review DMARC reports**
	1. It can take up to 48 hours for reports to start coming in.
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
		1. Quarantine is a little safer since mail is delivered but sent to junk.
		2. As mentioned before, it will take up to 48 hours for the changes to fully propagate.
	2. You can also add a `pct=` tag to only apply the policy to a percentage of unauthenticated mail
		1. For example, `pct=30` would only apply the policy to 30% of messages who fail SPF and DKIM authentication checks.
	3. You can now remove the `fo=1` tag from the DMARC record because you should have a very good idea about how your mail is authenticated and you only care about if mail isn't being delivered.
	4. Here's an example of the updated record which applies the Quarantine policy to 50% of messages that fail authentication.
		1. `v=DMARC1; p=quarantine; pct=50; rua=mailto:[aggregate report email address]; ruf=mailto:[failure report email address]`



<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/spf/#spf-implementation" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### SPF Implementation
Honestly, [the syntax guide on Open-SPF is phenomenal](http://www.open-spf.org/SPF_Record_Syntax/), but here's a breakdown of a typical record^[Gussied up for use as an example.] for quick reference.

The SPF record is processed in order; as soon as an email is matched to one of the rules, it stops getting processed and passes or fails the check.


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
	3. `mx`
		1. Check the current domain's DNS for MX records and mark them as designated senders
			1. **Note**: Since the default mechanism is `+`, it does not need to be explicitly written out
	4. `ip4:123.45.67.89`
		1. Designates the IP address `123.45.67.89` as an authorized sender
	5. `a:contoso.com`
		1. Checks `contoso.com` for A records^[Remember that A records are IPv4 addresses.] and designates them as authentic senders
	6. `include:mail.example.com`
		1. Checks `mail.example.com` for its own SPF record and includes that in the SPF record lookup
		2. Be careful as this can bring you much closer to the *10 DNS lookup limit* and cause a `PermError`
	7. `-all`
		1. Fail all originating hosts that have not been matched to this point in the record
			1. Because this is the last entry, any email that is authenticated by one of the earlier entries will get through, and everything else will fail authentication
			2. This is similar to Firewall configurations where the last rule is often `deny any any`
		2. Because this is a *Fail* qualifier, it has to be manually written out as `-`
			1. `~all` is also frequently seen in default configurations, and is used when transitioning between services or when using email marketing services (Mailchimp, Sendgrid, etc.) which do not get added to the SPF record.




</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dkim/#dkim-implementation-3rd-party-mail-provider" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DKIM Implementation: 3rd-Party Mail Provider
The process to configure DKIM is different for each email provider.

If your mail provider is also your DNS host, it's often as easy as checking a box. However, if your mail provider and DNS host are different, then you will be required to create specific DNS entries on the name server to authenticate. [Fastmail](https://www.fastmail.help/hc/en-us/articles/360058753354-Adding-MX-records-to-Namecheap#signing), for example, requires you to add three CNAME entries to your DNS host for authentication; this allows them to manage key rotation without bothering clients.

While it's best to follow the instructions provided by your mail provider, here's a quick-and-dirty overview of the DKIM record format. More tags can be found at [the DKIM Verification section on Wikipedia](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail#Verification)

1. **Name**: `mx01._domainkey.example.com`
	1. `mx01`
		1. This is the selector used to identify the correct key
		2. In this case, it appears to be the public key for the "MX01" mail server. The "MX02" mail server might have the selector `mx02`.
	2. `_domainkey`
		1. This identifies the TXT record as a DKIM (Domain Key) entry
	3. `example.com`
		1. The domain being checked.
		2. This may be input automatically, depending on your DNS host.
2. **Value**: `v=DKIM1; k=rsa; p=bG9sIHlvdSBhYnNvbHV0ZSBuZXJkLCB5b3UgZm91bmQgbWUhIEkgd2lzaCBJIGNvdWxkIGdpdmUgeW91IHNvbWV0aGluZywgYnV0IGFsYXM7IGhpdCBtZSB1cCBpZiB5b3Ugd2FudCB0byBjaGF0IQ==`
	1. `v=DKIM1`
		1. Specifies the DKIM version; at this point, it's always DKIM1
	2. `;`
		1. The separator between values
	3. `k=rsa`
		1. The *key type* specifies the kind of encryption used to create the key-pair
		2. The default is RSA
	4. `p=bG9sIHl...`
		1. The Base64-encoded *public key*, which is absolutely required 


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dmarc/#dmarc-implementation" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DMARC Implementation
Configuring DMARC is easy, but can cause you the most headaches because it's what authorizes email to be delivered, and a misconfiguration can stop your email in its tracks. Therefore, it's highly recommended that you *first configure your DMARC policy to take no action* on emails for the first couple of weeks, using the reports generated to make sure everything is getting delivered as expected, and then to add a `quarantine` or `reject` policy and maybe ramp up implementation through the `pct` tag.

Below is an example of a DMARC TXT record:

1. **Name**: `_dmarc.example.com`
	1. `_dmarc`
		1. Signifies this a DMARC TXT entry
	2. `example.com`
		1. The root/organization-level domain the policy is being applied to.
		2. Only one DMARC record needs to exist for the whole domain, but you can add more records for different subdomains to take different actions.
2. **Value**: `v=DMARC1; p=reject; sp=none; pct=100; aspf=r; adkim=r; rua=mailto:dmarc-reports@example.com; ruf=mailto:dmarc-failures@example.com; fo=1;`
	1. `v=DMARC1`
		1. DMARC version 1; at present, there is only one version.
	2. `;`
		1. The separator between tags.
		2. Spaces and tabs can make entries more readable, but are completely optional.
	3. `p=reject`
		1. The *Policy* applied to emails which fail their SPF and DKIM authentication checks
		2. There are three options:
			1. `none`: No action is taken
				1. Typically used while configuring email security and collecting reports.
			2. `quarantine`: Emails which fail authentication should be treated with suspicion and sent to the spam/junk folder
				1. This allows the recipient server to still receive and process unauthenticated mail, just treats them with suspicion
			3. `reject`: Instructs the receiving server to outright reject any mail that fails authentication.
				1. This is the most secure, but can also be the most problematic if a configuration changes or something goes wrong.
	4. `sp=quarantine`
		1. The policy for subdomains.
		2. If `sp` is not stated in the record, then the policy described by `p` applies to subdomains.
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
		2. Aggregate reports contain basic information and include successful and failed delivery information
	9. `ruf=mailto:dmarc-failures@example.com`
		1. Identifies the email address to which recipient servers should send *individual delivery forensic failure reports*
		2. Forensic failure reports contain detailed information about failed deliveries to assist with triage and troubleshooting.
	10. `fo=1`
		1. This generates a forensic report *for any SPF or DKIM failure*, which is helpful for triage and troubleshooting during initial setup.
			1. Default is `fo=0`, and once the `p` value is set to `quarantine` or `reject`, it can be safely removed.
		2. *Reminder*: many mailbox providers don't send forensic/failure reports for [[Definitions and Topics/GDPR\|GDPR]] compliance.

> If you are sending DMARC reports to another domain, you will need to create a TXT record on that domain's name server to identify each sending domain.^[[DMARC - DMARC External Validation](https://mxtoolbox.com/problem/dmarc/dmarc-external-validation?page=prob_dmarc&action=dmarc:annmulhern.com&showlogin=1&hidepitch=0&hidetoc=1)]
> 
> For example:
> 1. Type: `TXT`
> 2. Name: `sendingdomain.com._report._dmarc.receivingdomain.com`
> 3. Value: `"v=DMARC1"`
> 
> The TXT record **name** identifies the domain generating the report (`sendingdomain.com`), followed by `._report._dmarc` and the domain receiving the reports (`.receivingdomain.com`). 
> The TXT record **value** just identifies the DMARC version (`"v=DMARC1"`)
> 
> **WARNING**: While you can use a `*` wildcard to simplify the record to `*._report._dmarc.receivingdomain.com`, this allows anyone to send email to your DMARC report inboxes.
> Spam filters and firewalls don't typically inspect DMARC reports, and an attacker could exploit this to flood the inbox with bogus DMARC reports or inject malicious code into zipped attachments, which might get run automatically by report analyzing software. 




</div></div>



# Tools and Resources
## Resources
- [Learn and Test DMARC](https://www.dmarctester.com/)
	- Great site for quickly testing mail you can send
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
- [Free Domain Analyzer Tool \| PowerAnalyzer](https://powerdmarc.com/analyzer/)
	- Can automatically detect/select the DKIM selector
	- Checks BIMI and a few others with only a couple of clicks
- [GitHub - QbDVision-Inc/DMARC-Report-Analyzer: Analysis on your DMARC report files](https://github.com/QbDVision-Inc/DMARC-Report-Analyzer?)
	- Local, Python-based DMARC report analyzer
	- Can automatically log in to email accounts and download DMARC reports directly for analysis
- [Email authentication in Microsoft 365 - Microsoft Defender for Office 365 \| Microsoft Learn](https://learn.microsoft.com/en-us/defender-office-365/email-authentication-about)
	- Microsoft has a robust guide on configuring SPF, DKIM, and DMARC records for your organization.
- [Email Authentication Best Practices - Cisco](https://www.cisco.com/c/dam/en/us/products/collateral/security/esa-spf-dkim-dmarc.pdf)
	- I'm linking this document really just to rant; this "most optimal" "best practices" guide from Cisco is kind of crazy.
	- It may be helpful for larger environments, but their implementation strategy feels backwards to me.
		- It takes *6 and a half months* to get to the point where you're enacting policy, and you don't even setup a DMARC record until about the *third month*.
		- I think it's important, critical even, to be diligent when working with a production environment, and if you want to slow-roll deployment to make sure nothing goes wrong, all power to you, but to only start gathering live information half-way through deployment seems crazy to me.
## DNS Look-up tools
[[dig\|dig]]
[[nslookup\|nslookup]]