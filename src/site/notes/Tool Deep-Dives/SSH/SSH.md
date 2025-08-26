---
{"dg-publish":true,"permalink":"/tool-deep-dives/ssh/ssh/","updated":"2025-07-04T12:08:21.144-07:00"}
---

#### SSH
- *SSH (Secure Shell protocol)* "is a cryptographic network protocol for operating network services securely over an unsecured network."^[[Secure Shell - Wikipedia](https://en.wikipedia.org/wiki/Secure_Shell)]
- [[Tool Deep-Dives/SSH/OpenSSH\|OpenSSH]] is one of the key tools


### Usage
1. `ssh [username]@[hostname/target address]`
	1. `-p [port]`
		1. Target port address (default 22, but can be configured to something else)
	2. `-i [path to private key]`
		1. Used when authenticating with private keys instead of/in addition to passwords
	3. `-vvv`
		1. Verbose mode
2. `logout` or `exit`
	1. Terminate the session.
3. `ssh-keygen`
4. `ssh-agent`
	1. Keymanager for SSH; in my experience, it's critical to some apps being able to connect.
	2. You use `ssh-add` to add keys to the agent with `ssh-add \path\to\key\file`
   






# Metadata

### Sources
[Secure Shell - Wikipedia](https://en.wikipedia.org/wiki/Secure_Shell)
[It's 2023. You Should Be Using an Ed25519 SSH Key (And Other Current Best Practices) - Brandon Checketts](https://www.brandonchecketts.com/archives/its-2023-you-should-be-using-an-ed25519-ssh-key-and-other-current-best-practices)
[SSH Agent Explained](https://smallstep.com/blog/ssh-agent-explained/)
### Tags
#tools_ssh
