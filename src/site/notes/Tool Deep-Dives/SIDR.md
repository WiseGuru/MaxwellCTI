---
{"dg-publish":true,"permalink":"/tool-deep-dives/sidr/"}
---

[GitHub - strozfriedberg/sidr: Search Index Database Reporter](https://github.com/strozfriedberg/sidr)

Really neat application that outputs the Windows Search Index for Windows 10 (and earlier) ESE DB (Extensible Storage Engine) and Windows 11+ SQLite DB

- The DB can be dumped into files (CSV or JSON) or straight to shell


While I was watching the seminar [[BHIS Webinars/For the Love of the Windows Search Index with Alissa Torres\|For the Love of the Windows Search Index with Alissa Torres]], user **h,k** asked about Windows Search indexing file content for Markdown (.md) files, specifically relating to Obsidian.

I love Obsidian, so I wanted to give it a shot.

I modified the Advanced settings of Search Index (Settings>Privacy & Security>Searching Windows>Advanced indexing options>Advanced>File Types, "Index Properties and File Contents"), rebuilt my index, and checked it with SIDR.

There is a character limit for the file-content output somewhere around 1028 characters, though I'm not sure if it's in the Windows.db file or a quirk of SIDR. This appeared in both the CSV and terminal outputs from SIDR, and I'm not familiar enough with SQLite to check against the database itself at the moment, but interesting that it captures so much information!