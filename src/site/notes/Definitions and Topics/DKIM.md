---
{"dg-publish":true,"permalink":"/definitions-and-topics/dkim/","noteIcon":""}
---

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

# Metadata

### Sources
[DomainKeys Identified Mail (DKIM)](https://dkim.org/)
[DomainKeys Identified Mail - Wikipedia](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail)
### Tags
#defs_sec 