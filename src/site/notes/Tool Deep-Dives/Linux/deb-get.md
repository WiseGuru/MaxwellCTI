---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/deb-get/"}
---

#### deb-get
- [deb-get](https://github.com/wimpysworld/deb-get) is a tool that provides `apt-get` functionality for 3rd party repos and direct-download .deb files.
- *Critical to use a GitHub Personal Access Token to avoid [rate limiting issues](https://github.com/wimpysworld/deb-get?tab=readme-ov-file#github-api-rate-limits)*

It's honestly awesome, and I wish I'd known about it earlier; super easy to use, great for installing tools like Zoom or [Obsidian](https://help.obsidian.md/Getting+started/Download+and+install+Obsidian) or [Resilio Sync](https://help.resilio.com/hc/en-us/articles/206178924-Installing-Sync-package-on-Linux). It just requires a little setup to work perfectly.

### Commands
Generally, commands follow `apt-get`
1. `deb-get list`
	1. Show all available apps for installation through `deb-get`
	2. Use `--installed` to show only installed apps, and `--not-installed` to show all apps not installed.
2. `deb-get search <app>`
	1. Find an app with the string in part of the name
		1. e.g., `deb-get search pass` returns `1password`, `enpass`, and `keepassxc`

### Setup
#### Install
Follow the [GitHub instructions](https://github.com/wimpysworld/deb-get?tab=readme-ov-file#install) to download and install `deb-get`.

#### GitHub Personal Access Token (PAT)
1. Create a GitHub [Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).
	1. Click on your Profile Icon (top right)>Settings>Developer Settings (bottom left)>Personal access tokens (left)>Fine-grained tokens>Generate new token
	2. Configure the PAT
		1. Name
		2. Expiration (up to 1-year out)
		3. When selecting repository access, just set it to "Public Repositories (read-only)".
		4. Generate token
2. Insert it into your current working session
	1. Open your terminal and run `export DEBGET_TOKEN=github_pat_whatever-your-token-is`
3. Add it to your `.profile` or `.zshrc` config file (depending on if you use ZSH as your shell)
	1. The following commands will append the token to the config file; be sure you know which one you're using. 
	2. *Be careful that you don't share your .profile or .zshrc file with anyone or on GitHub!*
	3. `echo "export DEBGET_TOKEN=github_pat_whatever-your-token-is" >> ~/.profile`
	4. `echo "export DEBGET_TOKEN=github_pat_whatever-your-token-is" >> ~/.zshrc`
		1. If you use Oh My ZSH, run `omz reload` to re-run the config and load the token

##### Verify your token is working
First, verify the token is set correctly:
1. `echo $DEBGET_TOKEN`
2. If you see your token, then the variable is set; if you don't, check your prior commands or run the `export DEBGET...` command manually to add it to the environment

Second, test that it's functional:
- Per [the answer to this closed issue](https://github.com/wimpysworld/deb-get/issues/660#issuecomment-1314562812), you can run the following command to verify it's working:

```bash
# Query GitHub to check validity of GitHub PAT token.
curl \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer ${DEBGET_TOKEN}" \
  https://api.github.com/rate_limit
```

If you get back a bunch of JSON objects, you're golden!
The objects are formatted like this:

```bash
...
  "rate": {
    "limit": 5000,
    "used": 0,
    "remaining": 5000,
    "reset": 1726008283
  }
...
```

If the token is misconfigured, you'll see this:
```bash
{
  "message": "Bad credentials",
  "documentation_url": "https://docs.github.com/rest",
  "status": "401"
}
```



# Metadata

### Sources
[GitHub - wimpysworld/deb-get: apt-get for .debs published via GitHub or direct download 📦](https://github.com/wimpysworld/deb-get)
### Tags
#tools_linux 