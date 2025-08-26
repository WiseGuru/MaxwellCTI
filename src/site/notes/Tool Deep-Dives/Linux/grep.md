---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/grep/","tags":["tools_linux"],"updated":"2025-07-12T15:18:55.903-07:00"}
---

#### grep
- CLI utility for searching text data sets for matching expressions.
- You can use `\|` to separate "OR" strings
	- For example, `grep '445\|CLOSED\|ESTABLISHED' firewalloutput.txt`
- Important options
	- `-i` or `--ignore-case`
		- Search for string, regardless of case
		- Useful when adversaries use alternating caps to prevent search matches
	- `-v` or `--invert-match`
		- Searches for any results that do NOT include the search term
	- `-n` or `--line-number`
		- Get the line number with the output
	- `-l` or `--files-with-matches`
		- Just lists file names with matching content
- [[Tool Deep-Dives/Windows/Windows\|Windows]] alternatives
	- In [[Tool Deep-Dives/Windows/PowerShell\|PowerShell]]: `Select-String`
	- In Windows command line ([[Tool Deep-Dives/Windows/cmd.exe\|cmd.exe]]), `findstr`


Simple file copy script using [[for\|for]]
```shell
for file in $(grep -l "search_string" /files/to/sea.rch); do
  cp "$file" /path/to/destination/
done
```


# Metadata

### Sources
[grep - Wikipedia](https://en.wikipedia.org/wiki/Grep#:~:text=grep%20is%20a%20command%2Dline,which%20has%20the%20same%20effect.)