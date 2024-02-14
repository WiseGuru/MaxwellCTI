---
{"dg-publish":true,"permalink":"/cybersecurity-journey/soc-definitions/principle-of-least-privilege/","tags":["defs_soc"]}
---

#### Least Privilege
- The *Principle of Least Privilege* is the process of assigning only the permissions that are necessary and regularly auditing accounts to keep *privilege creep* in check
	- **Privilege Creep** is when a user's business function changes and permissions they no longer require are not removed from their account.
- The absolute basics
	- No local admin access
	- Create separate accounts for elevated access
		- e.g., if a user or administrator needs access to a server, create a separate account to access that server
			- Create multiple accounts for multiple server
	- Regularly audit accounts
		- Do users still need access to these folders?
		- When was the last time this admin/server account was logged in?
- Advanced/Super cool things to do
	- Configure admin accounts to cycle the password after each login
		- I don't remember the name of the tool I've used that did this, but basically you would log into a portal, and then select the server you wanted to access, and it would negotiate the session. When you logged off, the password for the account would automatically be reset
		- [[Cybersecurity Journey/SOC Definitions/LAPS\|Microsoft LAPS]] allows privileged users to access a managed local-admin account for user devices, and allows them to easily expire and cycle the password.
			- More tuned for managing user devices, and not perfect, since a password may not expire correctly if it's not connected to the network directly or via a VPN, but it can get the job done
	- Continuously monitor admin accounts for strange activity
	- Install infrastructure that allows users to run permitted escalated tasks on a need-to-perform basis






# Metadata

### Sources