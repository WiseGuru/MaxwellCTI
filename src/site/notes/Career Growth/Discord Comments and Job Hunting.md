---
{"dg-publish":true,"permalink":"/career-growth/discord-comments-and-job-hunting/"}
---

During the [[BHIS Antisyphon and Webinars/BlackHills SOC Core/BHIS SOCC Notes Overview\|SOC Core]] course, there were lots of interesting side-tracks that I wanted to record for posterity
# Various Home Lab Projects
These are a range of home lab guides and overviews that looked interesting; I will come back and summarize/organize them.
[Building Blue Team Home Lab Part 10 - SIEM Part 1 | facyber](https://facyber.me/posts/blue-team-lab-guide-part-10/)
[vulnerable-AD-plus](https://github.com/WaterExecution/vulnerable-AD-plus "https://github.com/WaterExecution/vulnerable-AD-plus") 
[0xBallpoiont/LOAD](https://github.com/0xBallpoint/LOAD "https://github.com/0xBallpoint/LOAD") 
[Orange-Cyberdefense/GOAD](https://github.com/Orange-Cyberdefense/GOAD "https://github.com/Orange-Cyberdefense/GOAD") 
[Active-Directory-Setup](https://github.com/Lavender-exe/Active-Directory-Setup "https://github.com/Lavender-exe/Active-Directory-Setup")

[Offensive Driver Development](https://training.zeropointsecurity.co.uk/courses/offensive-driver-development)
[Building a detection engineering lab](https://www.jmhatch.com/landing/blog/scholarly-project-part-1-building-a-detection-engineering-home-lab)
[AC-Hunter™ Community Edition - Active Countermeasures](https://www.activecountermeasures.com/ac-hunter-community-edition/)

[GitHub - JPCERTCC/LogonTracer: Investigate malicious Windows logon by visualizing and analyzing Windows event log](https://github.com/JPCERTCC/LogonTracer)


[Windows Security Log Encyclopedia](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx)

Go through ALL of the EDR tools
1. [this Cybersecurity Platform is FREE - YouTube](https://www.youtube.com/watch?v=i68atPbB8uQ)
2. Velociraptor
3. Elastic


<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">



# Job hunting

## Day 1

Jason Blanchard gave a talk about Job Hunting, and my short-hand notes are below. I've also linked below several videos of his that cover the topic in varying degrees, and I strongly recommend checking them out.

0h38m - [Building Resumes using Job Descriptions with Jason Blanchard - YouTube](https://www.youtube.com/watch?v=SB9uVUav4jI)
1h20m - [AASLR: Job Hunting Like a Hacker with Jason Blanchard - YouTube](https://www.youtube.com/watch?v=LThxETdRxlQ)
2h00m - [Infosec Job Hunting (Part 1 of 5): How to Locate the Work You Want - YouTube](https://www.youtube.com/watch?v=T_Zfzzl2sPg)

- Humans suck at describing themselves, they're much better at finding something that describes themselves
- People who are searching for jobs are low in self-confidence and high in self-doubt
	- When you see things that you feel are beyond your experience level
		- 3 levels of knowledge
			- 1. **Familiar** with, **experience** with, **knowledge** of, **conceptual understanding** of
				- I've heard of that before, I've taken a class or webcast on it, *I kind of know what that is*
			- 2. **Proficient** in
				- I use it on a daily or semi-daily basis
				- *I know hotkeys or shortcut keys*
			- 3. **Expert** in, *SME* (subject matter expert)
				- I know this so well, I could teach it to someone else
		- *Job hunting is not the time to downplay your expertise*
- Come up with things the describe who you are
	1. Create a new document named "Catch-all Resume"
	2. Search for SOC analyst (or the job you want)
		- `soc analyst -senior -sr -lead` (the senior/lead positions)
		- You can use `+[string]` to add a requirement
	3. Check the skills required, and copy/paste the bullets that connect with you into the new document
		- ![Day 1-1.png](/img/user/Attachments/Day%201-1.png)
		- Go through each bullet and customize it to fit your experience
	4. *ONCE* you have everything saved in one place, go to your LinkedIn
		- Add the bullets to the positions where you learned those things
		- Add a gap between the bullet points to make it easier for humans to read
- Recruiters and HR pros are "Magical Job Fairies"
	- They're successful if they fill a position
	- Reach out!
		- If there are recruiters/HR folks, reach out to them and say "Hey, I just applied for this position, and I wanted to see if you had 10-15 minutes to talk about it. *If there's anything else you need from me, please let me know.*
			- Always leave it with *your hands out to give*, not with your hands out *to take*
				- What does this mean?
			- Not all of them like it, they might say no, but that's fine
				- If they are assholes, then that saves you a ton of time, because it's clear they're assholes, and you can stop wasting time with them
- How to handle certifications
	- They help the HR professional who doesn't know the job
	- Go after the certs that are free or that you can afford
		- You can do "OSCP, comparable to GPEN" to bypass the poorly-configure ATS system
	- *CISSP opens more doors than any other certification*
- Cover Letters Should Die (according to Jason Blanchard)
	- Cover letters were going the customization part when the *resume* was the same
	- Now it's duplicative, because you're customizing the resume with the self descriptions
- Follow up schedule to increase likelihood of responses
	- 7/14/30
		- Reach out at 7 days
			- "Hi, I wanted to introduce myself; I applied recently, and I wanted to let you know that I'm available if you have any questions."
		- Reach out at 14 days
			- "Hey, I'm just reaching out to see if I can answer any questions or provide you with anything else to help in your decision."
		- Reach out at 30 days
			- "Hey, I'm sure you already filled the position, but I was curious if you had any feedback for me and hope you consider me for future positions."
- Use THM and HTB to learn, and then the CyberRange to demonstrate
- *Hi, I'm transitioning from System Administration, and I was wondering if you had some time to talk about how you got to where you are in your career?*
	- If they talk with you, they will tell you about their experience
		- They will probably give you other contacts to reach out to
	- End the conversation with "*My main two take-aways are...*
		- This tells them that you were listening, paying attention, etc., and you're going to take action
		- More valuable
- As often as possible, use "We" statements
	- I already do this, so should be easy
	- Distributes the credit to the team, and it makes you less of an asshole

## Day 2
1. Interview tips
	1. Ask clarifying questions
		1. Interviewer: "How would investigate logs in a windows environment?"
			1. "Can I ask clarifying questions? Is Sysmon configured, etc. etc."

## Day 3
1. Ideally, build your resume for that job posting and only that job posting
	1. Once you find the job, search for other positions at the company
		1. Network engineer, etc.
	2. Look for tools they use in their environment
		1. Palo Alto networks, Cisco, React, etc.
	3. Do some OSINT on those tools
		1. Check OWASP, CIS Benchmarks, etc. to see what's going on
		2. Check Shodan
		3. Check their website for details
			1. Java, IBM, F5, etc.
	4. Mark on your resume something to the effect of "Understanding of securing Palo Alto network devices" or "Understanding of React security best practices"
	5. DON'T EVER GO INTO AN INTERVIEW AND TELL THEM THEIR SHIT IS BROKEN
	6. Where does this go on your resume?
		1. ![Comments and Interview Job stuff-1.png](/img/user/Attachments/Comments%20and%20Interview%20Job%20stuff-1.png)
		2. ![Comments and Interview Job stuff-2.png](/img/user/Attachments/Comments%20and%20Interview%20Job%20stuff-2.png)
		3. ![Comments and Interview Job stuff-3.png](/img/user/Attachments/Comments%20and%20Interview%20Job%20stuff-3.png)
		4. [[Career Growth/Jon Hatch's Guide to Job Hunting\|Jon Hatch's Guide to Job Hunting]] also has a resume template that I think was quite good.
2. Informational interviews
	1. [Informational Interviews - JM Hatch's Cybersecurity Blog](https://www.jmhatch.com/landing/blog/informational-interviews)

</div></div>


### Building trust in an organization and presenting to bigwigs
1. Brown-bag meetings/lunch and learn
	1. Focus on user-actionable things
		1. Ghostery, secure home network, privacy, etc.
	2. This humanizes and demystifies some of security stuff
2. Bigwigs
	1. Prepare slides that cover the most actionable items
		1. slides are pretty, easy, etc.
	2. Predict detailed questions they're going to ask
		1. Prepare secondary slides that really go in detail
			1. "Why Security Onion? Why not something else?"
			2. *whips out slides* "Well, we could get locked into Cisco ecosystem, etc. etc."
	3. Dive into those questions and answer them as quickly as you can, then return to the main slides

## Various roadmaps
SOC Analyst Roadmap to Success [https://tylerwall.medium.com/soc-analyst-roadmap-to-success-ca07941370d8](https://tylerwall.medium.com/soc-analyst-roadmap-to-success-ca07941370d8 "https://tylerwall.medium.com/soc-analyst-roadmap-to-success-ca07941370d8") 
ChatGPT for SOC Analysts [https://medium.com/@tylerwall/chatgpt-for-soc-analysts-e86389340dcd](https://medium.com/@tylerwall/chatgpt-for-soc-analysts-e86389340dcd "https://medium.com/@tylerwall/chatgpt-for-soc-analysts-e86389340dcd") 
30m Azure Honeypot Project [https://tylerwall.medium.com/creating-an-azure-honeypot-2c2eeb89bc9e](https://tylerwall.medium.com/creating-an-azure-honeypot-2c2eeb89bc9e "https://tylerwall.medium.com/creating-an-azure-honeypot-2c2eeb89bc9e") 
SOC Analyst JOB Hunting [https://tylerwall.medium.com/soc-analyst-job-hunting-16e6e5b06d8a](https://tylerwall.medium.com/soc-analyst-job-hunting-16e6e5b06d8a "https://tylerwall.medium.com/soc-analyst-job-hunting-16e6e5b06d8a") 
SOC Analyst Prerequisite Skills [https://tylerwall.medium.com/soc-analyst-prerequiste-skills-dc6a3e4f92b7](https://tylerwall.medium.com/soc-analyst-prerequiste-skills-dc6a3e4f92b7 "https://tylerwall.medium.com/soc-analyst-prerequiste-skills-dc6a3e4f92b7") 
SOC Analyst Tools, Concepts & More [https://tylerwall.medium.com/soc-analyst-tools-concepts-more-8ad97f596beb?sk=720d5edc89cd3fcc375448ad973bad6f](https://tylerwall.medium.com/soc-analyst-tools-concepts-more-8ad97f596beb?sk=720d5edc89cd3fcc375448ad973bad6f "https://tylerwall.medium.com/soc-analyst-tools-concepts-more-8ad97f596beb?sk=720d5edc89cd3fcc375448ad973bad6f")

## [[Career Growth/Jon Hatch's Guide to Job Hunting\|Jon H's guide to getting hired]]
1. Tons of networking
2. References [Jason Blanchard's Job Hunting talk](https://www.youtube.com/watch?v=LThxETdRxlQ)
	1. [Antisyphon: Infosec Job Hunting - Building Resumes using Job Descriptions with Jason Blanchard - YouTube](https://www.youtube.com/watch?v=SB9uVUav4jI)

## Home Lab
