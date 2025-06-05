---
{"dg-publish":true,"permalink":"/definitions-and-topics/dkim/","noteIcon":""}
---

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

#### DKIM Implementation: 3rd-Party Mail Provider
The process to configure DKIM is different for each email provider.

If your mail provider is also your DNS host, it's often as easy as checking a box. However, if your mail provider and DNS host are different, then you will be required to create specific DNS entries on the name server to authenticate. [Fastmail](https://www.fastmail.help/hc/en-us/articles/360058753354-Adding-MX-records-to-Namecheap#signing), for example, requires you to add three CNAME entries to your DNS host for authentication.

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

# Metadata

### Sources
[DomainKeys Identified Mail (DKIM)](https://dkim.org/)
[DomainKeys Identified Mail - Wikipedia](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail)
[RFC 6376 - DomainKeys Identified Mail (DKIM) Signatures](https://datatracker.ietf.org/doc/html/rfc6376)
### Tags
#defs_sec 