---
{"dg-publish":true,"permalink":"/blog-and-research/zoom-phishing-email/"}
---

A client received an unusual phishing email from a zoom.us domain. The email looks like a bog-standard voicemail-type phishing email, and the "Play voicemail.mp3" button takes you to `hxxps[://]8xw[.]77c[.]mytemp[.]website/voicemailzoom.html#victim@domain[.]com`.

![Zoom Phishing Email-1.png](/img/user/Attachments/Zoom%20Phishing%20Email-1.png)

It's pretty common to receive phishing emails with "Zoom" as the sender's name or somewhere in the header, but this actually came from their domain. Zoom has a strong [[Definitions and Topics/DMARC\|DMARC]] policy of `p=reject`, and if this email wasn't sent by Zoom, it should have been rejected. This means one of two things:
1. The attacker was able to craft a custom email using Zoom.
2. The receiving server is not honoring Zoom's DMARC policy.

I use the "DKIM Verifier" extension for Thunderbird, so we can tell immediately that this email isn't authenticated with [[Definitions and Topics/DKIM\|DKIM]],^[The default configuration on appears with valid DKIM records; select DKIM>Options>Display, and change "Show DKIM header" to "when an email is viewer" to see the DKIM record for all emails.] and checking the headers shows the original Brazilian sending domain and IP:

```
Received: from 45[.]178[.]113[.]187[.]seegfibras[.]com[.]br ([45[.]178[.]113[.]187]) by instance-us-central1-4frm[.]prod[.]antispam[.]mailspamprotection[.]com
```

This means that the receiving email server is not honoring Zoom's "reject" DMARC policy. Following this, I wanted to see for myself how easy it was to bypass a DMARC `reject` policy; I sent several test emails using [Emkei's Fake Mailer](https://emkei.cz/) to Fastmail, Gmail, Siteground, and Yahoo. 

> [!Sidenote]
> In my tests with Emkei, the email's "From" name was set to "Zoom," but was changed on delivery to "972-449-1241 via Zoom", which matches the original phishing email. This appears to be a Zoom meeting ID, and the attacker in the original email used the number in the subject and email body to look like a phone number.

Both Siteground and Fastmail quarantined a spoofed Zoom email in junk instead of rejecting it. Gmail has a history of rejecting all email from Emkei, and Yahoo correctly rejected the spoofing email from Zoom. Yahoo also appears to all email from Emkei with a high-level of suspicion as even without a DMARC policy but sent from Emkei are quarantined in junk.

Testing with other domains revealed similar results, though Siteground ended up being a little more irregular on whether an email was delivered or not. 

Not following a domain's DMARC policy is dangerous; Zoom and other heavy-traffic domains have a high risk of being spoofed and high level of trust from users. Unwary or unsavvy users might think that a legitimate email from Zoom was sent to junk by accident and click the link anyway.
