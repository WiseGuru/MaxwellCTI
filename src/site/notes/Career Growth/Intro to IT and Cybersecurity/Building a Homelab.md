---
{"dg-publish":true,"permalink":"/career-growth/intro-to-it-and-cybersecurity/building-a-homelab/","tags":["VM"],"noteIcon":""}
---

# Why Build a Homelab?
Building a homelab is critical for someone new to IT; it gives you a safe environment to tinker and meddle and break and fix things.

A homelab can be as simple or complicated as you like; for the purpose of getting started in IT, however, there are two things I recommend; *virtual machines* and *old cheap or free hardware*.

## Virtual Machines
**Virtual machines** are a great place to get started; you can learn about Microsoft's Windows Servers, Active Directory, and create "user" computers to join to the domain. It can also be a safe place to try out configurations, learn about *group policy*, system resource management, and many other things.

The best is that, as long as you already have a computer,^[ARM processors, like Apple Silicon, have required special tooling to work with traditional Windows VMs; however, Parallels and (I believe) Virtualbox both support running x86_64 virtual machines on ARM CPUs, but I have not tested this yet.] you can get started right now and for free.

## Old Hardware
**Old hardware** is another great way to start; many offices run on Dell Optiplex desktops, Lenovo Thinkpad laptops, or HP... computers, and being able to actually look at and work with the hardware that's even a few generations gives you a rough idea of what's being used. Additionally, most big companies have hardware lifecycles that run around 3 to 5 years, meaning that every year, thousands of computers that are 3 to 5 years old get sent to ewaste recycling centers or are "rescued" by members of the IT team. 

Disassembling, recombining, and upgrading these old machines is an education in itself, and you will become intimately familiar with how the computer works and gain confidence in tinkering with any system. Besides, it's not critical if something breaks since it's not your primary computer, and you will learn a lot in trying to fix it.

> If you need inspiration, the Linus Tech Tips [Scrapyard Wars series](https://www.youtube.com/watch?v=x1JA24KCAjE&list=PL8mG-RkN2uTyuEutQa79RZ0Q5u5gteUci) is a fun lesson in building competent computers from old and cobbled together hardware.

If you do not have a spare monitor or TV you can dedicate to a desktop, you can install a hypervisor like [[Tool Deep-Dives/Proxmox/Proxmox Virtual Environment\|Proxmox Virtual Environment]] and access it remotely from another computer, tablet, or smartphone.^[Though managing multiple OSes from a smartphone will be rough.]
## Do Both
The best thing is to do both; get an old desktop, install Windows or Linux, and then run your homelab in a virtual environment. Or, if you get several low-powered computers, physically build out your homelab with one OS per computer.

# Getting Old Hardware
When looking for or buying old hardware, here are some rules of thumb;

1. *Free is free*
	1. If you're getting a free computer, accept it with gratitude and figure out what to do with it later.
	2. You can run many operating systems on computers with old or low hardware specs, and you don't need the latest and greatest.
	3. Only really worry about the following points if you're paying for a computer or if you have a specific task in mind.
2. CPU Generations Matters
	1. For a long time, Intel believed that people only needed a few cores to do their work; however, once AMD got serious with Ryzen, Intel had to pick up the pace and add cores.
		1. The more cores and threads^[Cores are the physical processing cores on the CPU, and threads are virtual cores. Applications and processes are often locked to certain cores, so the more cores and threads you have, the more multitasking you can do on the system.] you have and the newer the CPU, the more you can do with the machine.
	2. Intel guidelines
		1. Look for Core i7 CPUs
			1. Core i5s are fine, but they often don't include hyperthreading, so you're stuck with the physical core count.
			2. 3rd to 7th gen CPUs run 4 cores/8 threads.
			3. Starting on the 8th gen, they upgrade it to 6 cores/12 threads, which gives you more lee-way to run more virtual machines.
		2. I would recommend paying up to $100 for a desktop with a 6th or 7th gen Core i7, or $150-250 for a desktop with an 8th gen Core i7
			1. You will see linear performance improvements from Intel's 3rd to 7th gen Core i7 CPUs, but the same number of cores.
			2. I wouldn't recommend *buying*^[But free is free.] 5th gen or earlier, and I wouldn't pay more than $100 for a desktop with a 6th gen Core i7.
	3. AMD Guidelines
		1. Pre-Ryzen, AMD CPUs were not good.
			1. Don't *buy* ANY desktop with a non-Ryzen AMD CPU in it.^[But free is free!]
		2. Aim for a Ryzen 5 or Ryzen 7, any generation
			1. The lowest tier 1st gen Ryzen 5 is 4 cores/8 threads, and the Ryzen 7 is 8 cores/16 threads; *tons of power for virtual machines*
		3. These are a bit rarer because they were not adopted as quickly, but you can get *screaming* deals on them.
			1. A Ryzen 5 1600 *narrowly loses in performance* to the Intel Core i7-7700 released the same year, and Dell Optiplex desktops can be had for less than $100. 
		4. Verify that it has a GPU or has integrated graphics
			1. None of the 1st gen Ryzen CPUs have built in graphics capabilities; this is completely fine, you just will want to make sure that there is a graphics card (GPU) included in the system.
				1. Since the desktops were sold as a complete unit, most listings include the discrete GPU, but some have removed it; if it's missing, you will need to source another one.
				2. [[Career Growth/Intro to IT and Cybersecurity/Building a Homelab#GPU vs. No GPU\|Here are some examples of systems with and without GPUs]]
			2. 2nd gen and newer Ryzen CPUs *may* have integrated graphics; these models are appended with a "G" after the model number.
				1. For example, "Ryzen 5 2400GE" has integrated graphics where "Ryzen 2600E" does not.

There are a ton of places you can get old hardware, and here's the rough path I would take to get started.

1. Ask people you know if they have an old computer lying around.
	1. Some people have old computers they replaced years ago and didn't want to get rid of because of the perceived value, but now it's gathering dust and taking up space. Help them out by asking if you can take it off their hands!
	2. As I mentioned earlier, people who work in IT have a tendency to collect odd pieces of tech, or know of old computers that would be better used in your hands than stacked in a closet. Even if the computer is broken, you will learn a lot by troubleshooting it, finding replacement parts, and fixing it.
2. Check local ewaste centers and computer recycling companies
	1. Computer recycling companies often have stacks of computers they need to process, and are often happy to sell them to you directly.
	2. If you can get a few broken computers of the same model for cheap, you can probably cobble them together to get one working computer.
		1. If you do get broken computers, make sure they haven't been fully harvested of working parts; make sure they have RAM and CPUs, and if they're missing hard drives,^[Which is normal for ewaste companies who offer data destruction services.] see if you can get some used/wiped drives for cheap.
3. Shop local online marketplaces
	1. Craigslist, Facebook Marketplace, and others are a great place to start.
	2. Make sure that you can test the hardware to verify that it boots before buying it.
4. Ebay
	1. You can get some great deals, but you want to make sure that you're getting a good deal; some companies will try to hide important pieces of information to make the computer sound more enticing than it is.
		1. For example, they will advertise a desktop with "An Intel i7 processor" and only mention in the small text of the item description that it's a 2nd gen i7. For context, we're expecting Intel 15th gen processors in 2025.

# Guides
Here are some great guides on getting started with your homelab; the first set is a group of videos about getting hardware and getting started, and the second list is more like tutorials and using your homelab.

I recommend pairing this with the [[Career Growth/Intro to IT and Cybersecurity/Recommended Helpdesk Classes and Lectures\|Recommended Helpdesk Classes and Lectures]] to combine the lectures with hands on experience in your lab.

1. Intro videos - getting hardware etc. (may be some redundancy)
	1. [Your Old PC is Your New Server - YouTube](https://www.youtube.com/watch?v=zPmqbtKwtgw)
	2. [The $0 Home Server - YouTube](https://www.youtube.com/watch?v=IuRWqzfX1ik)
	3. [Cheap Homelab Setup for under $100! - YouTube](https://www.youtube.com/watch?v=ubOMgmTQjfs)
2. Homelab Setup Walkthroughs
	1. [IT: Helpdesk (How To Make Your Own Lab At Home?) - YouTube](https://www.youtube.com/watch?v=5xt7KR8eENE)
	2. [How to Setup a Basic Home Lab Running Active Directory (Oracle VirtualBox) | Add Users w/PowerShell - YouTube](https://www.youtube.com/watch?v=MHsI8hJmggI)
	3. [ACTIVE DIRECTORY #00 Creating our Server + Workstation Virtual Environment - YouTube](https://www.youtube.com/watch?v=pKtDQtsubio&list=PL1H1sBF1VAKVoU6Q2u7BBGPsnkn-rajlp)


# GPU vs. No GPU

## GPU
![Building a Homelab-1.png](/img/user/Attachments/Building%20a%20Homelab-1.png)

![Building a Homelab-2.png](/img/user/Attachments/Building%20a%20Homelab-2.png)

## No GPU
![Building a Homelab-GPU-2.png](/img/user/Attachments/Building%20a%20Homelab-GPU-2.png)

![Building a Homelab-GPU-1.png](/img/user/Attachments/Building%20a%20Homelab-GPU-1.png)