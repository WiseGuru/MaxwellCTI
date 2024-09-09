---
{"dg-publish":true,"permalink":"/webinars-and-training/black-hills-soc-core/labs/bhis-socc-lab-firewall-log/"}
---


## LAB: Server Logs Analysis
[IntroLabs/IntroClassFiles/Tools/IntroClass/FirewallLog/FirewallLog.md at master · strandjs/IntroLabs · GitHub](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/Tools/IntroClass/FirewallLog/FirewallLog.md)

In this lab, we're analyzing logs from a Cisco ASA firewall. The output uses non-standard/Cisco standard [Syslog](https://ccnadefinitions.com/ccna/20-definitions/syslog/) formatting, and includes a lot of extraneous information.

![BHIS-SOCC-lab-FirewallLog.png](/img/user/Attachments/BHIS-SOCC-lab-FirewallLog.png)
> Sample of the output, unfiltered.

To start, here are the tools we're familiar with, and some that are new.

1. Tools
	1. [[Tool Deep-Dives/Linux/grep\|grep]]
		1. Only display lines with a specific string
		2. You can pipe `grep` multiple times to narrow down the search
		3. `-v` - invert-match, meaning it will EXCLUDE those results
	2. [[Tool Deep-Dives/Linux/less\|less]]
		1. Only show a page of results at a time
		2. `q` - quit
		3. `arrow up/down` - scroll by line
		4. `space` - page down
	3. [[Tool Deep-Dives/Linux/cut\|cut]]
		1. Select for specific text positions in a table
		2. `-d` - DELIMITER; select the delimiter
		3. `-f` - FIELDS; only select specific fields
	4. [[Tool Deep-Dives/Linux/R core\|R core]], or `rscript`
		1. A tool for performing math on the output
		2. It's kind of a whole deal, but I'll explain what happens in this use case later on
2. Analyzing the logs
	1. Who is who?
		1. 192.168.1.1 is the host (firewall) system
		2. 192.168.1.6 is the workstation we're interested in
		3. 24.230.56.6 is the local gateway
	2. Let's use that information to clear out the logs
		1. `grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | less`
		2. Once that's cleared out, we see LOTS of teardown packets from a couple of external IPs to just our suspicious host.
	3. Let's eliminate extraneous information, and narrow down our search; let's tag on the cut command
		1. `... | cut -d' ' -f 1,3-5,7-14 | less`
			1. This spread gives us the date, event, and the message
			2. We can use hyphens (`3-5`, `7-14`) to include all fields in between
			3. What we see are tons of teardowns of TCP connections, with similar looking numbers for each remote host, 18.160..174 and 13.107..38
				1. If we add `| grep <remote host IP> |`, we see the package sizes have very minimal variance
	5. In summary, here's what we see when we isolate the logs to TCP teardowns.
		1. 1-second durations
		2. packet sizes are the same
		3. All with a couple IP addresses
	6. Alarm bells should already be going off, but let's add some [[Tool Deep-Dives/Linux/R core\|rscript]] to further analyze the output
	7. Let's add some math on those byte sizes: `| Rscript -e 'scan("stdin", quiet=TRUE)-> y' -e 'cat(min(y), max(y), mean(y), sd(y), var(y), sep="\n")'`
		1. Let's break it down:
			1. `Rscript` 
				1. This is just the [[Tool Deep-Dives/Linux/R core\|rscript]] command
			2. `-e 'scan("stdin", quiet=TRUE)-> y'`
				1. The first expression is in single quotes
				2. `scan("stdin", quiet=TRUE)`
					1. Scans the standard input for information, and does not print that information to the console
				3. `-> y`
					1. The scan is sent to the `y` variable
				4. I've been a little cheeky here; the lab originally runs this in the opposite direction, `y <-scan(...)`
					1. Apparently, this is the original way to do things, but I find this less readable and intuitive, so modified it here
			3. `-e 'cat(min(y), max(y), mean(y), sd(y), var(y), sep="\n")'`
				1. This expression takes the `y` variable we just assigned and runs various operations against it
					1. It calculates and prints the minimum, maximum, mean, standard deviation, and variance of these numbers, in that order.
				2. `cat` is used to concatenate^[Concatenate: link (things) together in a chain or series.] the output for the following functions
				3. `min`(imum), `max`(imum), `mean`, `sd` (standard deviation), and `var`(iance) are calculated from `y`
				4. `sep="\n"` separates each calculation with a new line
	8. Here's the full command for each (with some deviance from the lab for seemingly irrelevant commands)
		1. `grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | grep FIN | grep 18.160.185.174 | cut -d ' ' -f 14 | Rscript -e 'scan("stdin", quiet=TRUE)-> y' -e 'cat(min(y), max(y), mean(y), sd(y), var(y), sep="\n")'`
			1. ![BHIS-SOCC-lab-FirewallLog-1.png](/img/user/Attachments/BHIS-SOCC-lab-FirewallLog-1.png)
			2. There's very little variance or deviation; the packets are clustered really tightly around the mean of about 1829 bytes.
		2. `grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | grep FIN | grep 13.107.237.38 | cut -d ' ' -f 14 | Rscript -e 'scan("stdin", quiet=TRUE)-> y' -e 'cat(min(y), max(y), mean(y), sd(y), var(y), sep="\n")'`
			1. ![BHIS-SOCC-lab-FirewallLog-2.png](/img/user/Attachments/BHIS-SOCC-lab-FirewallLog-2.png)
			2. This has a lot more deviation, but is still very concerning.
3. Final analysis
	1. Doesn't feel human
		1. Humans are messy and chaotic, this feels automated.
		2. Indicates communication with a pair of [[Definitions and Topics/C2\|C2]] servers.