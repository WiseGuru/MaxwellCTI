---
{"dg-publish":true,"permalink":"/definitions-and-topics/biba-integrity-model/"}
---

#### Biba Integrity Model
- Designed to ensure the *Integrity* of documents and information by restricting the flow of information *down*
	- e.g., restricting unauthorized people from making changes to files above their *integrity* (*trust*) level
- Kind of the opposite of the [[Definitions and Topics/Bell-LaPadula Model\|Bell-LaPadula Model]], but instead of *Authorization* levels, it's *Data Integrity* levels
- it's a "read up, write down" model
	- *Write down*: Users can only *create* content at or *below* their integrity level
		- e.g., a *System Administrator* can write how-to guides for general users, but a Helpdesk technician is not permitted to modify the [[Definitions and Topics/Business Continuity and Disaster Recovery Guide\|Business Continuity and Disaster Recovery Guide]]
	- *Read up*: Users can only *read* content at or *above* their integrity level
		- e.g., A Helpdesk technician can read instructions provided by a SysAdmin, but should ignore instructions from a user^[The internet is down, reboot the datacenter!]






# Metadata

### Sources
[Biba Model - Wikipedia](https://en.wikipedia.org/wiki/Biba_Model)

### Tags
#defs_sec 