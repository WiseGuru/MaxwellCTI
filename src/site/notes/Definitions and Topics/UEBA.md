---
{"dg-publish":true,"permalink":"/definitions-and-topics/ueba/","updated":"2024-03-13T08:44:59.000-07:00"}
---

#### UEBA
- *User and Entity Behavior Analytics* (*UEBA*) aggregates logs and analyses behavior.
- Works by assigning values to events, and sending alerts once a certain threshold is reached
	- Example config:
		- Successful user login is +1
		- User logoff is -1
		- Unsuccessful user login is +2
		- Threshold is 6
			- If a user logs in 6 times successfully or fails to login 3 times, an alert is generated
- Requires significant effort to tune and configure a [[Definitions and Topics/baselining\|baseline]]
	- Sometimes has a bad-rap with sysadmins and defenders because it can be so effortful
		- Not plug-and-play





# Metadata

### Sources


### Tags
#defs_sec 