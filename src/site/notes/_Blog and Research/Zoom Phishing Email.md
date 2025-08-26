---
{"dg-publish":true,"permalink":"/blog-and-research/zoom-phishing-email/","updated":"2025-08-06T12:46:09.000-07:00"}
---

# Zoom Gets Spoofed

A client received an unusual phishing email from a zoom.us domain. The email looks like a bog-standard voicemail-type phishing email, and the "Play voicemail.mp3" button takes you to `hxxps[://]8xw[.]77c[.]mytemp[.]website/voicemailzoom.html#victim@domain[.]com`.

![Zoom Phishing Email-1.png](/img/user/Attachments/Zoom%20Phishing%20Email-1.png)

It's pretty common to receive phishing emails with "Zoom" as the sender's name or somewhere in the header, but this actually came from their domain. Zoom has a strong [[Definitions and Topics/DMARC\|DMARC]] policy of `p=reject`, and if this email wasn't sent by Zoom, it should have been rejected. This means one of two things:
1. The attacker was able to craft a custom email using Zoom.
2. The receiving server is not honoring Zoom's DMARC policy.

I use the "DKIM Verifier" extension for Thunderbird, so we can tell immediately that this email isn't authenticated with [[Definitions and Topics/DKIM\|DKIM]],^[The default configuration only appears with valid DKIM records; select DKIM>Options>Display, and change "Show DKIM header" to "when an email is viewer" to see the DKIM record for all emails.] and checking the headers shows the original Brazilian sending domain and IP:

```
Received: from 45[.]178[.]113[.]187[.]seegfibras[.]com[.]br ([45[.]178[.]113[.]187]) by instance-us-central1-4frm[.]prod[.]antispam[.]mailspamprotection[.]com
```

To me, this means that the receiving email server is not honoring Zoom's "reject" DMARC policy. But that can't be right, right? No modern email service would fail to honor DMARC, right?

# Sending Test Spoof Emails

Following this, I wanted to see for myself how easy it was to bypass a DMARC `reject` policy; I sent several test emails using [Emkei's Fake Mailer](https://emkei.cz/) to Fastmail, Gmail, Siteground, and Yahoo. 

> [!Sidenote]
> In my tests with Emkei, the email's "From" name was set to "Zoom," but was changed on delivery to "972-449-1241 via Zoom", which matches the original phishing email. This appears to be a Zoom meeting ID, and the attacker in the original email used the number in the subject and email body to look like a phone number.

Both Gmail and Yahoo rejected the spoofed email, and Siteground and Fastmail quarantined it. Gmail has a history of rejecting all email from Emkei's Fake Mailer, and Yahoo correctly rejected the spoofing email from Zoom.

Testing with other domains revealed similar results; Yahoo sent spoofed emails from domains without DMARC policies to junk and Fastmail junked all spoofs which failed DMARC (though delivered spoofs without DMARC). 
Siteground ended up being a little more irregular on whether an email was delivered or not; some emails from domains with a `reject` policy got through where others did not. 

I reached out to Siteground and Fastmail support for comment.
# Fastmail Support

Fastmail indicated that this was by design because users are dumb and belligerent (my paraphrase). While I don't agree, I understand the rationale, and at least they send DMARC reports so that domain administrators have some visibility.

![Zoom Phishing Email-2.png](/img/user/Attachments/Zoom%20Phishing%20Email-2.png)

# Siteground Support

Siteground Support did their best to evade all responsibility for supporting DMARC.

When I first contacted them over the phone, they didn't create a ticket. A day later when the client received another Zoom spoofed email, I contacted them again; they created finally created a ticket and then one of their technicians responded to it.

In the first email, they acknowledged the Zoom email was a spoof, then said I should tighten the SPF/DMARC records for my domains by using `-all` for SPF and `p=reject` for DMARC to protect against spoofing.

> [!danger]- First Email from Siteground Support
> Hello (Client),
> 
> Thank you for contacting us today!
> 
> Thanks again for bringing the spoofed Zoom email to our attention. We’ve completed a thorough analysis and confirmed that:
> 
> - It was not actually sent by Zoom, despite appearing to be from no-reply@zoom.us.
> 
> - The email failed both SPF and DMARC, meaning it was unauthorized.
> 
> - It originated from an IP address unrelated to Zoom.
> 
> To better protect against spoofing and phishing in the future, we recommend the following DNS record updates:
> 
> Current SPF record:
> 
> `v=spf1 include:... ~all`
> 
> Change to this (if you're sure no other systems send email on your behalf):
> 
> `v=spf1 include:... -all`
> 
> This change tells other servers to reject emails from senders not explicitly allowed in your SPF record.
> 
> Current DMARC record:
> 
> `v=DMARC1; p=quarantine; rua=mailto:dmarc_aggregate@domain.com; ruf=mailto:dmarc_forensic@domain.com`
> 
> Recommended update:
> 
> `v=DMARC1; p=reject; sp=reject; adkim=s; aspf=s; rua=mailto:dmarc_aggregate@domain.com; ruf=mailto:dmarc_forensic@domain.com; fo=1`
> 
> This changes can be done via Site Tools, section Domain and then DNS Zone editor. If you’d prefer, we can assist with making these changes or review them once updated.
> 
> If you need further assistance, you are more than welcome to contact us again at any time. 
 
I said thanks, but that wasn't my question; it was how can we protect against incoming spoofed emails?

> [!faq]- My first reply
> Thank you for getting back to me! I appreciate the information on configuring SPF, and DMARC for domains I control; however, the question is how the spoofed Zoom email got through Siteground's email firewall, not about emails from my domain getting spoofed.
> 
> Zoom has a DMARC policy set to "reject", meaning that this email shouldn't have been delivered at all. Does Siteground not honor sender "reject" policies?

They replied with basically the same thing as the first time; the key difference is that made a strange claim; "While SiteGround's email system is designed to filter out unauthorized emails, there are instances where spoofed emails can slip through due to the complexity of email routing and the evolving tactics used by spammers. The DMARC policy set by Zoom is indeed _"reject"_, which should prevent unauthorized emails from being delivered. However, there are scenarios where intermediate mail servers do not strictly enforce these policies, leading to delivery."

> [!danger]- Second Email from Siteground Support
> We understand how frustrating it can be to receive such emails, especially when security protocols like DMARC are in place.
> 
> The email you received was indeed spoofed and not sent by Zoom, despite appearing to come from no-reply@zoom.us. Our analysis confirmed that the email failed both SPF (Sender Policy Framework) and DMARC (Domain-based Message Authentication, Reporting & Conformance) checks, which means it was not authorized by Zoom. The email originated from an IP address unrelated to Zoom, which is a common tactic used in email spoofing.
> 
> While SiteGround's email system is designed to filter out unauthorized emails, there are instances where spoofed emails can slip through due to the complexity of email routing and the evolving tactics used by spammers. The DMARC policy set by Zoom is indeed "reject", which should prevent unauthorized emails from being delivered. However, there are scenarios where intermediate mail servers do not strictly enforce these policies, leading to delivery.
> 
> Like my colleague, I would recommend updating your domain's SPF and DMARC records to strengthen your defenses against spoofing. The changes that he previously suggested can help ensure that emails appearing to come from your domain are properly authenticated.
> 
> Recommended updates:
> 
> **SPF:**
> 
> `v=spf1 include:domain.com.spf.auto.dnssmarthost.net -all`
> 
> **DMARC:**
> 
> `v=DMARC1; p=reject; sp=reject; adkim=s; aspf=s; rua=mailto:dmarc_aggregate@domain.com; ruf=mailto:dmarc_forensic@domain.com; fo=1`
> 
> You can make the recommended DNS changes via Site Tools in the DNS Zone Editor section. If you prefer, our team can assist with these updates or review them once you've made the changes.
> 
> If you encounter the same issue and continue receiving similar emails after modifying your SPF and DKIM records, please feel free to reach out to us so we can further inspect the email headers and the available logs on our end.
> 
> Thank you for your understanding and cooperation.

I was kind of annoyed with this, and so called their support directly; the response I got was "there's nothing we can do about this for external domains, but if you send us headers from domains you own, we can take additional steps."

# Testing Siteground

So I figured why not, let me tighten up the domains for my client; I won't be able to apply the SPF changes to domains used for newsletters (since SPF fails automatically there), but let's see what happens. After making the changes, Siteground was rejecting spoofed emails correctly! Hooray!

At this point, I was pretty confident that Siteground wasn't honoring DMARC policies, but I wanted to be sure. I picked one of the domains with the shortest-lived SPF and DMARC DNS records, shrunk them down to 5 minutes, and began sending spoof emails from that domain with different policy in four different combinations between SPF softfail/hardfail and DMARC reject/quarantine.

Looking at the headers of received spoof emails, only emails that arrived when the domain was set with SPF `~all` (softfail) were delivered.

I wanted to recheck how Zoom's email got through, and sure enough, their SPF record ends in a softfail. I checked another similar company, RingCentral, and their SPF ended in a hardfail; sending a spoof from RingCentral was rejected by Siteground, but Zoom was consistently delivered (albeit to junk).

I'm still in a back-and-forth with Siteground support; whether they want to admit it or not, it's clear Siteground doesn't respect DMARC, and that's risky.

# Why this is a problem

Email authentication is hard; it's why many companies never get around to it, and why there is an entire industry around email authentication configuration and reporting. It's been iterated on for over 20 years, starting with the [[Definitions and Topics/SPF\|SPF]] in the mid-2000s, [[Definitions and Topics/DKIM\|DKIM]] soon after, and [[Definitions and Topics/DMARC\|DMARC]] in 2015.

SPF was designed to both list authorized senders and prescribe policy; kind of like a proto-DMARC. Things get complicated, however, when you add services like MailChimp or SendGrid, which actually should not be configured with SPF, or when someone configures email forwarding. If a receiving domain only enforced SPF policy, `-all` would immediately reject any forwarded email and any authentic newsletters you signed up for. Additionally, SPF lacks the reporting features of DMARC, leaving sending domains in the dark when it comes to trends in spoofing. Implementing DKIM and DMARC solves these problems, but if the receiving email server isn't configured to check them, then they're relying entirely on SPF.

Zoom isn't doing anything wrong here; SPF is considered part of the authentication process for a reason, and it's frankly the one most likely to be bunged up with email forwarding or other issues. Zoom invitations are often sent across domains, to distribution lists, to forwarded email addresses, and having a strict SPF policy would cause significant friction for recipients with improperly configured email security tools. RingCentral is primarily a VoIP provider and likely doesn't have the same friction points (or they've accepted the risk).

And some might be asking "If this spoof email was delivered to junk anyway, why should we care?" We should care because plenty legitimate emails get delivered to Junk all the time, and many users have been trained to look in Junk for authentic mail that slipped through. An unwary or unsavvy user could easily be fooled into clicking a link if they check the sending domain and don't realize it's been spoofed.

# Key Takeaways

I am currently experimenting with email filtering rules for the client to better isolate or reject emails which fail DMARC; it won't have the nuance of actually applying the DMARC policy, but it's a step in the right direction.

As I've mentioned before, I recommend using subdomains for anything which cannot be protected with an SPF `-all` record (such as newsletters and marketing) and to configure `-all` for all domains which can support it. If a mail server is configured to honor DMARC, then it should handle the email based on its DMARC policy, not SPF. However, if a mail is not able to honor DMARC, then it should fall back on SPF, and the `-all` will protect your domain from impersonation and your recipients from phishing. However, some authentic mail may not be delivered and you will have no visibility into it.

How you choose to handle this is up to you and your risk appetite; most modern email hosts respect DMARC, and having an SPF final `~all` or `-all` shouldn't impact mail deliverability. However, if the recipient uses a cheap webhost as their email provider, they are open to phishing emails for any "misconfigured" SPF record.

# Final Thoughts

Email servers not respecting a domain's DMARC policy is dangerous; Zoom and other heavy-traffic domains have a high risk of being spoofed and a high level of trust from users. It's important for domains to take appropriate steps to protect their domains from phishing, but improper configurations on receiving servers undermines those security efforts.