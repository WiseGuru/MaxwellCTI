---
{"dg-publish":true,"permalink":"/tool-deep-dives/python/dmarc-report-analyzer/","noteIcon":""}
---

#### DMARC Report Analyzer
- *DMARC Report Analyzer* is a tool written in [[Tool Deep-Dives/Python/Python\|Python]] that allows you to analyze all DMARC reports in a folder or inbox and generate a spreadsheet with the results.
- I've forked a version and added^[With the help of an LLM.] various features, including more progress bars, an offline cache for the Spamhaus list of IPs, and an output CSV which includes more information that can be helpful when troubleshooting [[Technical Guides/Securing Email\|email authentication issues.]]

If working on a [[Tool Deep-Dives/Linux/Linux\|Linux]] system, you will need to use a [[Tool Deep-Dives/Python/Python Virtual Environment\|Python Virtual Environment]] to download and update the correct packages. This is easily managed with a shell script that performs the run/setup process.

```bash
#!/usr/bin/env bash
cd ~/scripts/DMARC-Report-Analyzer
# Optional
python3 -m venv env
source env/bin/activate  # On Windows use `env\Scripts\activate`
# Mandatory
pip install -r requirements.txt
python3 main.py
```



# Metadata

### Sources
- [GitHub - WiseGuru/DMARC-Report-Analyzer: Analysis on your DMARC report files](https://github.com/WiseGuru/DMARC-Report-Analyzer/tree/main)
- [GitHub - QbDVision-Inc/DMARC-Report-Analyzer: Analysis on your DMARC report files](https://github.com/QbDVision-Inc/DMARC-Report-Analyzer)

### Tags
#tools_Python 