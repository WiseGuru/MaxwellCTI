---
{"dg-publish":true,"permalink":"/tool-deep-dives/ssh/open-ssh/","noteIcon":""}
---

#### OpenSSH
- *OpenSSH* "OpenSSH is a suite of secure networking utilities based on the [[Tool Deep-Dives/SSH/SSH\|Secure Shell protocol]], which provides a secure channel over an unsecured network in a client–server architecture."^[[OpenSSH - Wikipedia](https://en.wikipedia.org/wiki/OpenSSH)]


### Install and Configure OpenSSH on Windows 11

#### 1. Install the OpenSSH Server
1. **Open Windows Settings:**
   - Press `Win + I` to open the Settings app.
2. **Navigate to Optional Features:**
   - Go to *Apps > Optional Features*.
3. **Add an Optional Feature:**
   - Scroll down and click on *Add a feature*.
4. **Search for OpenSSH Server:**
   - In the search box, type *OpenSSH Server*.
   - Select OpenSSH Server from the list and click *Install*.

#### 2. Generate an ed25519 Key with a Passphrase on the Client (Windows and Linux)

**Why ed25519 and Why Add a Passphrase:**
- **ED25519** is preferred over RSA (even with a 4096-bit key) for several reasons:
  - **Security:** ED25519 is based on elliptic curve cryptography (vs. factoring large prime numbers like RSA) and is considered highly secure with a 256-bit key length.
  - **Performance:** ED25519 is faster in both key generation and authentication operations compared to RSA, making it more efficient.
  - **Simplicity:** ED25519 uses a fixed key size, simplifying key management.
- **Adding a Passphrase**:
  - A passphrase adds an additional layer of security by protecting the private key. If the private key file is compromised, the attacker would still need the passphrase to use it. This is particularly important on devices that may not always be physically secure.

1. **Open Preferred Terminal**
   1. Windows: Press `Win + X`, then select *Windows Terminal (Admin)* or *PowerShell (Admin)*.
   2. Linux: Whatever your preferred terminal emulator is.
2. **Generate the Key:**
   1. `ssh-keygen -t ed25519 -C [Identifying comment]`
	   1. `-t [algorithm]` choose the algorithm to use (e.g. rsa, ed25519, etc)
	   2. `-C [Comment]` used to identify the key
   2. Follow the prompts to save the key in the default location:
	   1. Windows: `C:\Users\YourUsername\.ssh\id_ed25519`
	   2. Linux: `~/.ssh/id_ed25519`
   3. When prompted, enter a strong passphrase.


**Transfer the Public Key to the Host Using `scp`:**

1. **From Windows:**
   ```powershell
   scp C:\Users\YourUsername\.ssh\id_ed25519.pub user@remote_host:/tmp/id_ed25519.pub
   ```

2. **From Linux:**
   ```bash
   scp ~/.ssh/id_ed25519.pub user@remote_host:/tmp/id_ed25519.pub
   ```

#### 3. Add the Key to Either the User or Administrator Key File and Fix Permissions

1. **SSH into the Remote Host:**
   ```bash
   ssh max@remote_host -p 22
   ```

2. **Move the Public Key to the Appropriate Location:**

**For a Standard User (e.g., `Max`):**
   ```bash
   mkdir -p ~/.ssh
   cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

**For an Administrator:**
   ```bash
   mkdir -p C:\ProgramData\ssh
   cat /tmp/id_ed25519.pub >> C:\ProgramData\ssh\administrators_authorized_keys
   ```

3. **Remove the Temporary Public Key File:**
   ```bash
   rm /tmp/id_ed25519.pub
   ```

#### 4. Configure the sshd_config File

1. **Open the `sshd_config` File:**
   - Use a text editor like `notepad` or `vim` to edit the file:
   ```powershell
   notepad C:\ProgramData\ssh\sshd_config
   ```

2. **Modify the Configuration:**

**Configure the Server to Use User or Administrator Keys:**

- **For User-Specific Keys:**
   ```plaintext
   AuthorizedKeysFile %h/.ssh/authorized_keys
   ```

- **For Administrator Keys:**
   ```plaintext
   Match Group administrators
       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
   ```

**Disable Password Authentication:**
   ```plaintext
   PasswordAuthentication no
   ```
   - Disabling password authentication forces the use of SSH key-based authentication, which is more secure.

**Change the Default Port**
Search the file for `port 22` and replace `22` with whatever port number you want between *1024* and *65536*

> changing the default port number prevents automated scanners from detecting an open SSH port and reduces the number of failed logins from automated services.

3. **Save and Close the File.**

4. **Restart the SSH Service:**

   **Manually:**
   1. Open Services:
      - Press `Win + R`, type `services.msc`, and press *Enter* to open the Services management console.
   2. Locate the OpenSSH Server Service:
      - Scroll down and find *OpenSSH SSH Server* in the list.
   3. Restart the Service:
      - Right-click on OpenSSH SSH Server and select *Restart*.

   **Using PowerShell:**
   ```powershell
   Restart-Service sshd
   ```

#### 5. Start the OpenSSH Server

1. **Manually:**
   1. Open Services:
      - Press `Win + R`, type `services.msc`, and press *Enter* to open the Services management console.
   2. Locate the OpenSSH Server Service:
      - Scroll down and find *OpenSSH SSH Server* in the list.
   3. Start the Service:
      - Right-click on OpenSSH SSH Server and select *Start*.
      - To ensure it starts automatically on boot, right-click the service, select Properties, and set the Startup type to Automatic.

2. **Using PowerShell:**
   ```powershell
   Set-Service sshd -StartupType Automatic -Status Running
   ```

#### 6. Configure the Windows Firewall

1. **Open Windows Defender Firewall:**
   - Search for Windows Defender Firewall with Advanced Security in the Start menu and open it.

2. **Create a New Inbound Rule:**
   1. In the left pane, select Inbound Rules.
   2. In the right pane, click New Rule....

3. **Configure the Rule:**
   1. **Rule Type:** Select Port and click Next.
   2. **Protocol and Ports:** Select TCP and specify Specific local ports as `22` (or the port you configured in `sshd_config`). Click Next.
   3. **Action:** Select Allow the connection. Click Next.
   4. **Profile:** Choose when the rule applies (Domain, Private, Public). Click Next.
   5. **Name:** Give your rule a name, such as OpenSSH Inbound Rule. Click Finish.


### Install OpenSSH Server on the target Ubuntu server
1. Install openssh-server (if not already installed)
	1. `sudo apt update && sudo apt install openssh-server`
2. Ensure openssh-server is running
	1. `sudo systemctl status ssh`
3. Ensure there is a permitting rule in the firewall
	1. `sudo ufw allow ssh`
4. Check the IP address of the server
	1. `ip a`



# Metadata

### Sources
[OpenSSH Server | Ubuntu](https://ubuntu.com/server/docs/service-openssh)
[How to enable SSH on Linux Ubuntu (Easy step by step guide) - YouTube](https://www.youtube.com/watch?v=Wlmne44M6fQ)
[Key-based authentication in OpenSSH for Windows | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement)
[SSH Key Algorithms: RSA vs ECDSA vs Ed25519 - VulnerX](https://vulnerx.com/ssh-key-algorithms/)
[It's 2023. You Should Be Using an Ed25519 SSH Key (And Other Current Best Practices) - Brandon Checketts](https://www.brandonchecketts.com/archives/its-2023-you-should-be-using-an-ed25519-ssh-key-and-other-current-best-practices)
### Tags
#tools_ssh 





