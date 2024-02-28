---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/grep/","tags":["tools_linux"]}
---

#### grep
- CLI utility for searching text data sets for matching expressions.
- Y\ou can use `\|` to separate "OR" strings
	- For example, `grep '445\|CLOSED\|ESTABLISHED'`
- Important options
	- `-i` or `--ignore-case`
		- Search for string, regardless of case
		- Useful when adversaries use alternating caps to prevent search matches
	- `-v` or `--invert-match`
		- Searches for any results that do NOT include the search term






# Metadata

### Sources
[grep - Wikipedia](https://en.wikipedia.org/wiki/Grep#:~:text=grep%20is%20a%20command%2Dline,which%20has%20the%20same%20effect.)