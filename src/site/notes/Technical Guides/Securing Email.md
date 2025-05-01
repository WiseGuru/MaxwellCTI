---
{"dg-publish":true,"permalink":"/technical-guides/securing-email/","noteIcon":""}
---

# Definitions

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/spf/#spf" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### SPF
- *SPF* (Sender Policy Framework) is a mechanism that identifies email servers that are authorized to send email from your domain.
	- Uses a TXT or SPF record on your DNS host/nameserver
	- Recipients perform a DNS lookup to confirm the sender.
	- If the SPF record is missing, or the sender is not authenticated, the message will fail and may not be delivered.
- Each sending mail server/domain must be identified
	- This can be directly via IP or through a domain lookup
		- SPF is limited to *10 DNS lookups*; going over 10 causes a `PermError`
	- It is not recommended to identify marketing services, like Mailchimp or Sendgrid, in SPF
		- Due to the high volume of email they send and the number of distinct email servers they use to get around spam restrictions, you can't identify an IP address and domain look-ups can timeout and cause problems for authentication
- Provides [[Definitions and Topics/AAA\|Authentication]] and [[Definitions and Topics/AAA\|Authorization]]


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dkim/#dkim" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DKIM
- *DKIM* (Domain Keys Identified Mail) is a mechanism that authenticates all messages sent from a domain using [[Definitions and Topics/PKI\|PKI (Public Key Infrastructure)]]
	- There are two keys; a private key used for signing, and a public key for verification
	- The administrator publishes the public key to the DNS record
	- The sending server signs all out-bound messages using the private key
	- Recipients verify the signed email against the public key
- The DKIM key is shared via a *TXT record*
	- DKIM records are differentiated with key selectors, and each email server requires its own public/private key pair.
		- You may have multiple DKIM records with different key selectors.
		- Because authentication occurs by key pair, tools like [MXToolbox](https://mxtoolbox.com/dkim.aspx) require the selector to test and validate the correct key.
	- Can also be configured as a CNAME record^[Canonical name, which functions as an alias and points to another address.] which points to the actual DKIM TXT record
- Provides [[Definitions and Topics/AAA\|Authentication]]


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dmarc/#dmarc" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DMARC
- *DMARC* (Domain-based Message Authentication, Reporting, and Conformance) is a mechanism that aligns tells a receiving email server what to do based on the [[Definitions and Topics/SPF\|SPF]] and [[Definitions and Topics/DKIM\|DKIM]] authentication checks
	- DMARC checks whether the domain the email's "From" field matches (or is aligned with) the SPF or DKIM authenticated domains.
	- If an email's "From" domain is aligned either the SPF or DKIM authenticated domains, then it can be delivered.
- DMARC Alignment can be "Relaxed" or "Strict"
	- Relaxed means email from a matching root-level (or organization level) domain will align
		- e.g., `marketing.example.com` will align with `example.com`
	- Strict means that the domain in the email must match exactly to the authenticated domain
		- e.g., `marketing.example.com` would fail to align with `example.com`
- There should only be one DMARC TXT record on your DNS host.
	- Like [[Definitions and Topics/SPF\|SPF]], it applies to all emails sent from your domain, and not to specific hosts like [[Definitions and Topics/DKIM\|DKIM]]
- DMARC can be configured in purely an audit mode without SPF and DKIM
	- No authentication or authorization is performed, and no action is taken, but you get reports on who is sending emails on your domain's behalf.


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


# Getting Setup

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/spf/#spf-implementation" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### SPF Implementation
Honestly, [the syntax guide on Open-SPF is phenomenal](http://www.open-spf.org/SPF_Record_Syntax/), but here's a breakdown of a typical record^[Gussied up for use as an example.] for quick reference.
The SPF record is processed in order; as soon as an email is matched to one of the rules, it stops getting processed and passes or fails the check.

The name of the SPF record designates the domain its being applied to; it can be `@` or the current domain to reference the domain itself (e.g. `example.com`) or a subdomain like `mail.example.com`.

1. Name: `@`
	1. `@`
		1. Designates the domain to which the SPF record applies; `@` is the current top-level domain (e.g., `example.com`)
		2. Can also use a subdomain, like `mail` or `mail.example.com`, with formatting depending on your DNS host.
2. Value: `v=spf1 +a mx ip4:123.45.67.89 a:contoso.com include:mail.example.com -all`
	1. `v=spf1`
		1. Version = SPF 1
	2. `+a`
		1. The `+` is a qualifier that says what to do if an email matches that value; here are a few of the most common ones.
			1. `+` = Pass, the host is allowed to send; this is the default, and does not need to be explicitly written out and I only put it here for demonstration purposes
			2. `-` = Fail, the host is not allowed to send
			3. `~` = SoftFail, transitioning away from the host and not allowed to send, but authentic mail still come from that host
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
		1. Fail All hosts that have not been matched to this point in the record
			1. Because this is the last entry, any mail that is authenticated by one of the earlier entries will get through, and everything else is failed
			2. Similar to Firewall configuration where the last rule is often `deny any any`
		2. Because this is a Fail qualifier, it has to be manually written out as `-`




</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dkim/#dkim-implementation-3rd-party-mail-provider" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DKIM Implementation: 3rd-Party Mail Provider
The process to configure DKIM between different mail hosts is often different. If your mail provider is also your DNS host, it's as easy as checking a box. However, if your mail provider and DNS host are different, then you will be required to create specific DNS entries on the name server to authenticate. [Fastmail](https://www.fastmail.help/hc/en-us/articles/360058753354-Adding-MX-records-to-Namecheap#signing), for example, requires you to add three CNAME entries to your DNS host for authentication; this allows them to manage key rotation without bothering clients.
While it's best to follow the instructions provided by your mail provider, here's a quick-and-dirty overview of DKIM format. More tags can be found at [the DKIM Verification section on Wikipedia](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail#Verification)

1. Name: `default._domainkey.example.com`
	1. `default`
		1. This is the selector used to identify the correct key.
	2. `_domainkey`
		1. This identifies the TXT record as a DKIM (Domain Key) entry
	3. `example.com`
		1. The domain being checked.
		2. This may be input automatically, depending on your DNS host.
2. Value: `v=DKIM1; k=rsa; p=bG9sIHlvdSBhYnNvbHV0ZSBuZXJkLCB5b3UgZm91bmQgbWUhIEkgd2lzaCBJIGNvdWxkIGdpdmUgeW91IHNvbWV0aGluZywgYnV0IGFsYXM7IGhpdCBtZSB1cCBpZiB5b3Ugd2FudCB0byBjaGF0IQ==`
	1. `v=DKIM1`
		1. Specifies the DKIM version; at this point, it's always DKIM1
	2. `;`
		1. The separator between values
	3. `k=rsa`
		1. The *key type* specifies the kind of encryption used to create the key-pair
		2. The default is RSA
	4. `p=bG9sIHl...`
		1. The Base64-encoded *public key*, absolutely required 


</div></div>


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/definitions-and-topics/dmarc/#dmarc-implementation" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### DMARC Implementation
Configuring DMARC is easy, but can cause you the most headaches because it's what authorizes email to be delivered, and a misconfiguration can stop your email in its tracks. Therefore, it's highly recommended that you first configure your DMARC policy to take no action on emails for the first couple of weeks, using the reports generated to make sure everything is getting delivered as expected, and then to ramp up implementation through the `pct` tag or take the gamble and go all in.

Below is an example of a DMARC TXT record:

1. Name: `_dmarc.example.com`
	1. `_dmarc`
		1. Signifies this a DMARC TXT entry
2. Value: `v=DMARC1; p=reject; sp=none; pct=100; aspf=r; adkim=r; rua=mailto:dmarc-reports@example.com; ruf=mailto:dmarc-failures@example.com;`
	1. `v=DMARC1`
		1. DMARC version 1; at present, there is only one version.
	2. `;`
		1. Separator between tags
	3. `p=reject`
		1. The *Policy* applied to emails which fail their SPF and DKIM authentication checks
		2. There are three options:
			1. `none`: No action is taken, typically used while configuring email security and collecting reports.
			2. `quarantine`: Emails which fail authentication should be treated with suspicion and sent to the spam/junk folder
				1. This allows the recipient server to still receive and process unauthenticated mail, just treats them with suspicion
			3. `reject`: Instructs the receiving server to outright reject any mail that fails authentication.
				1. This is the most secure, but can also be the most problematic if a configuration changes.
	4. `sp=quarantine`
		1. The policy for subdomains.
		2. If it's not identified in the record, then the policy described by `p` applies to subdomains.
	5. `pct=100`
		1. The percent of unauthenticated emails to apply the policy to
			1. e.g., `pct=20` would only apply the `p=reject` policy to 20% of emails which fail authentication
		2. This is helpful during a slow rollout to make sure not all email flow stops
	6. `aspf=r`
		1. SPF alignment requirements
			1. `s` is strict, and domains must match exactly
			2. `r` is relaxed, and only the root/organizational domain must match
	7. `adkim=r`
		1. DKIM alignment requirements
	8. `rua=mailto:dmarc-reports@example.com`
		1. Identifies the email address to which recipient servers should send *delivery aggregate reports*
		2. Aggregate reports contain basic information and include successful and failed delivery information
	9. `ruf=mailto:dmarc-failures@example.com`
		1. Identifies the email address to which recipient servers should send *individual delivery forensic failure reports*
		2. Forensic failure reports contain detailed information about failed deliveries to assist with triage and troubleshooting.




</div></div>


# Tools
[[dig\|dig]]
[[nslookup\|nslookup]]
[Learn and Test DMARC](https://www.dmarctester.com/)
[SPF Check & SPF Lookup - Sender Policy Framework (SPF) - MxToolBox](https://mxtoolbox.com/spf.aspx)
[DKIM Check- DomainKeys Identified Mail (DKIM) Record Lookup - MxToolBox](https://mxtoolbox.com/dkim.aspx)
[DMARC Check Tool - Domain Message Authentication Reporting & Conformance Lookup - MxToolBox](https://mxtoolbox.com/dmarc.aspx)
[Network Tools: DNS,IP,Email - MxToolBox](https://mxtoolbox.com/SuperTool.aspx)
[Free Domain Analyzer Tool \| PowerAnalyzer](https://powerdmarc.com/analyzer/)