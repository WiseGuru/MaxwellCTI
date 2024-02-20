---
{"dg-publish":true,"permalink":"/attack-frameworks/attack-surface-management/asm-resources/"}
---

## ASM Videos

Nahamsec - 10 minutes each there-about
[Attack Surface Management Series - EP0 - What is ASM (In under 10 mins) - YouTube](https://www.youtube.com/watch?v=sbkXpSeW77c)
[Attack Surface Management Series - EP1 - Certificate Transparency (In under 10 mins) - YouTube](https://www.youtube.com/watch?v=siELBLVW5cU&list=PLKAaMVNxvLmDtXJw7vEQjxbvdqeE3O0es&index=7)
[Attack Surface Management Series - EP2 - Shodan - YouTube](https://www.youtube.com/watch?v=D_wuxfwjZB0&list=PLKAaMVNxvLmDtXJw7vEQjxbvdqeE3O0es&index=6)

NahamSec series, doesn't include EP0
[Recon & Attack Surface Management - YouTube](https://www.youtube.com/playlist?list=PLKAaMVNxvLmDtXJw7vEQjxbvdqeE3O0es)

## BHIS [Ashley Knowles' video on ASM 101](https://www.youtube.com/watch?v=cznJlbEA2ws)
In this video, Ashley Knowles said she had a list of FOSS/Paid ASM services on her GitHub. So I investigated, and found that her current list^[The video is a few years old, she might have dropped her original one] was forked from another account several of the links were outdated. I decided to make my own fork with some notes and a datestamp:

[GitHub - Curated list of open-source & paid Attack Surface Monitoring (ASM) tools.](https://github.com/WiseGuru/awesome-attack-surface-monitoring)

The original list has a few issues/pull requests from other people, and while the creator has reacted to them, they haven't integrated those changes, leading me to believe the list is effectively static. Therefore, I'll probably just keep this list updated separately and modify it as it suits me.

## Random LinkedIn Post
This post listed a few tools and linked to a [CSO Online article](https://www.csoonline.com/article/570887/7-best-practices-for-enterprise-attack-surface-management.html). 

The original post feels a little wordy,^[I'm always succinct, concise, to the point, and never verbose, wordy, circumlocutory, or repetitive. That is to say, I don't repeat myself.] so here's my summary, with all the LinkedIn tracking links removed.

### Summary
[7 best practices for enterprise attack surface management | CSO Online](https://www.csoonline.com/article/570887/7-best-practices-for-enterprise-attack-surface-management.html)

When it comes to Attack Surface Management, don't be overwhelmed with expensive products. There are plenty of Free, Cheap, and Open Source products that will suit your needs.

I've broken these tools up into three categories (note that all of them except nmap, zenmap, ZAP, and Burp Suite CE are GitHub Repos, which we love to see).

1. Domain and Sub-domain Discovery  
	1. OWASP Amass is legit and does this well among other things: [amass: In-depth attack surface mapping and asset discovery](https://github.com/owasp-amass/amass)
	2. SubBrute is fast and unapologetic and has some nice upgrades: [subbrute: A DNS meta-query spider that enumerates DNS records, and subdomains.](https://github.com/TheRook/subbrute)
	3. Knock is simple and efficient: [knock: Knock Subdomain Scan](https://github.com/guelfoweb/knock)
2. Port Scanning (know your potential network ingress points)  
	1. nmap is the OG: [https://nmap.org/](https://nmap.org/)  
	2. zenmap is nmap's prettier sibling; just has a nice GUI and helpers: [https://nmap.org/zenmap/](https://nmap.org/zenmap/)  
3. Vulnerability/Configuration Scanning  
	1. nuclei is nifty and customizable: [nuclei: Fast and customizable vulnerability scanner based on simple YAML based DSL.](https://github.com/projectdiscovery/nuclei)
	2. ZAP is wonderful for web app scans: [https://www.zaproxy.org/](https://www.zaproxy.org/)  
	3. Burp is amazing for a million reasons: [Download Burp Suite Community Edition - PortSwigger](https://portswigger.net/burp/communitydownload)

### Original post
[Joshua Bregler on LinkedIn: 7 best practices for enterprise attack surface management](https://www.linkedin.com/posts/breglercissp_7-best-practices-for-enterprise-attack-surface-activity-7161741020362170369-Sopd?utm_source=share&utm_medium=member_desktop)

On Fridays, we build things... This week...  
  
Attack Surface Management!  
  
Understanding the external perspective of your organization is incredibly important. Comprehension of your organization's exposed digital real estate is important to understand your attackers' point-of-view.  
  
But the fact is... the products on the market that can do this for you may be out of your budget.  
  
But have hope! You can build this on your own. And it's a lot of fun!  
  
The [CSO Online](https://www.linkedin.com/company/csoonline/) article is great for framing the basic tenants: [https://lnkd.in/ec29vBF4](https://lnkd.in/ec29vBF4)  
  
Ultimately... you can strap together a few basic kinds of tools and get a decent idea of what your attackers see and start making changes to mitigate the issues.  
  
Here's some open source tools to get you started. This is by no means exhaustive and there is A LOT more you can do/add for your situation or industry. But start here and grow where it makes sense...  
  
- Domain and Sub-domain Discovery  
-- OWASP Amass is legit and does this well among other things: [https://lnkd.in/eSTt6Nhj](https://lnkd.in/eSTt6Nhj)  
-- SubBrute is fast and unapologetic and has some nice upgrades: [https://lnkd.in/ev2vZ5Jz](https://lnkd.in/ev2vZ5Jz)  
-- Knock is simple and efficient: [https://lnkd.in/eFKibXbf](https://lnkd.in/eFKibXbf)  
  
- Port Scanning (know your potential network ingress points)  
-- nmap is the OG: [https://nmap.org/](https://nmap.org/)  
-- zenmap is nmap's prettier sibling; just has a nice GUI and helpers: [https://nmap.org/zenmap/](https://nmap.org/zenmap/)  
  
- Vulnerability/Configuration Scanning  
-- nuclei is nifty and customizable: [https://lnkd.in/eFFTevQZ](https://lnkd.in/eFFTevQZ)  
-- ZAP is wonderful for web app scans: [https://www.zaproxy.org/](https://www.zaproxy.org/)  
-- Burp is amazing for a million reasons: [https://lnkd.in/eFbD_-Gd](https://lnkd.in/eFbD_-Gd)  
  
Is this basic? Yes. But you gotta get basic right before you do more.  
  
Build confidence in your exposed attack surface area. And have fun doing it!  
  
It's a beautiful day to build beautiful things.