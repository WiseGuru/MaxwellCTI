---
{"dg-publish":true,"permalink":"/tool-deep-dives/python/dmarc-report-analyzer/","noteIcon":""}
---

#### DMARC Report Analyzer
- *DMARC Report Analyzer* is a tool written in [[Tool Deep-Dives/Python/Python\|Python]] that allows you to analyze all DMARC reports in a folder or inbox and generate a spreadsheet with the results.
- I've forked a version and added^[With the help of an LLM.] various features, including more progress bars, an offline cache for the Spamhaus list of IPs, and an output CSV which includes more information that can be helpful when troubleshooting [[Technical Guides/Securing Email\|email authentication issues.]]

To install and use [DMARC Report Analyzer](https://github.com/WiseGuru/DMARC-Report-Analyzer/tree/main), there is a [helpful script in the Installation section](https://github.com/WiseGuru/DMARC-Report-Analyzer?tab=readme-ov-file#installation) to clone the repository to your local machine, create the local environment, install the requirements, and then run `main.py`. First, however, I recommend creating a `scripts` (or similar) folder somewhere to keep everything organized and then opening your Terminal from that folder.

The script expects you to have Python version 3 installed on your system and is currently written for Linux, but there is an option for Windows (which I haven't really tested). 

If working on a [[Tool Deep-Dives/Linux/Linux\|Linux]] system, you will need to use a [[Tool Deep-Dives/Python/Python Virtual Environment\|Python Virtual Environment]] to download and update the correct packages. This is easily managed with a shell script that performs the run/setup process.

```bash
#!/usr/bin/env bash
cd ~/scripts/DMARC-Report-Analyzer
# Optional
python3 -m venv env
source env/bin/activate
# Mandatory
pip install -r requirements.txt
python3 main.py
```

The final result is a spreadsheet with a summary of all of the reports it collected. You can use [[Tool Deep-Dives/whois\|whois]] and [[Tool Deep-Dives/Linux/grep\|grep]] to lookup the IP addresses to to find out where emails are coming from with the command `whois [IP address] | grep "NetRange:\|CIDR:\|Organization:"`, and your output should look something like this:

![DMARC Report Analyzer.png](/img/user/Attachments/DMARC%20Report%20Analyzer.png)



# Metadata

### Sources
- [GitHub - WiseGuru/DMARC-Report-Analyzer: Analysis on your DMARC report files](https://github.com/WiseGuru/DMARC-Report-Analyzer/tree/main)
- [GitHub - QbDVision-Inc/DMARC-Report-Analyzer: Analysis on your DMARC report files](https://github.com/QbDVision-Inc/DMARC-Report-Analyzer)

### Tags
#tools_Python 