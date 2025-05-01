---
{"dg-publish":true,"permalink":"/definitions-and-topics/dmarc/","noteIcon":""}
---

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



# Metadata

### Sources
[dmarc.org – Domain Message Authentication Reporting & Conformance](https://dmarc.org/)
[DMARC - Wikipedia](https://en.wikipedia.org/wiki/DMARC)
[Learn and Test DMARC](https://www.dmarctester.com/)

### Tags
#defs_sec 