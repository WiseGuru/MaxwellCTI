---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/upgrade-vs-dist-upgrade/","updated":"2024-01-03T12:49:44.000-08:00"}
---


#  tl;dr
- `apt upgrade` just upgrades all installed apps, and `apt dist-upgrade` installs apps and installs/removes dependencies
	- `dist-upgrade` might screw up other dependencies.


# Someone who knows what they're talking about

**Source**: [What are the risks of doing apt-get upgrade(s), but never apt-get dist-upgrade(s)? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/748000/what-are-the-risks-of-doing-apt-get-upgrades-but-never-apt-get-dist-upgrades)

> While 'apt upgrade' offers a conservative approach to upgrading packages without installing or removing any additional software, 'apt dist-upgrade' provides a more comprehensive solution, handling complex dependency changes and minimizing the impact on other packages.

The decision to perform a dist-upgrade is recommended for keeping your system up to date with the latest security patches and bug fixes **but on an unknown VM should be based on a careful assessment of the risks involved!**

A `dist-upgrade` may install new packages or remove existing.

If you decide to `dist-upgrade`, check to have the availability of a backup or a rollback, to restore the original state.

**Risks when Decide**:

- `dist-upgrade` may install new packages or remove them, this can lead to conflicts between packages, if there are custom configurations or thirdparty software installed on the system.
    
- It can introduce changes that may not be fully tested or compatible with your specific environment, this can result in system instability, crashes, or unpredictable behavior.
    
- If the vm has been customized or has a specific software, the configurations may overwriten or conflicts with those customizations and could lead to issues with the function.
    

**Risks when not decide:**

- By not upgrading the system components, you might miss out important security updates that could leave your system vulnerable to exploits and attacks.
    
- Newer packages and dependencies may not be compatible with older versions.
    
- You might miss out on bug fixes, performance improvements, and new features introduced in the updated packages.
    

[Why use apt-get upgrade instead of apt-get dist-upgrade?](https://askubuntu.com/questions/194651/why-use-apt-get-upgrade-instead-of-apt-get-dist-upgrade)

[Apt Upgrade vs Apt Dist-upgrade: The Key Differences](https://tecadmin.net/difference-between-apt-upgrade-vs-apt-dist-upgrade/)

[What's the difference between apt-get upgrade vs dist-upgrade?](https://itsfoss.com/apt-get-upgrade-vs-dist-upgrade/)