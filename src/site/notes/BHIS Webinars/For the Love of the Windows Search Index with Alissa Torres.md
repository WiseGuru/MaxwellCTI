---
{"dg-publish":true,"permalink":"/bhis-webinars/for-the-love-of-the-windows-search-index-with-alissa-torres/"}
---

### Introduction
I managed to catch this excellent seminar live, and I highly recommend it. Below are my notes taken from the class, my thoughts, and a link to the seminar (it has the deck in the description). 

[For the Love of the Windows Search Index w/ Alissa Torres - YouTube](https://www.youtube.com/watch?v=Ej37pJf1nw0)

### Notes

Microsoft used to use ESE, but now uses SQLlite


SearchIndexer.exe
- Hosts indexes and the list of URIs that require indexing
- Can go to "Advanced" and modify file types and what gets indexed
- Contains the query APIs that other applications use to leverage the Windows Search features
SearchProtocolHost.exe
- Hosts the protocol handlers
- Runs with least privileges required to index
SearchFilterHost.exe
- hosts the ifilters and property handlers to extract metadata and textual content.
- Low integrity process


Windows.edb
"Never trust anything in appdata"
- Windows.edb is in appdata; %appdata%\Windows\Windows.edb
- Primary database for Windows 10
- One database, three valuable tables
	- SystemIndex_Gth: 20 columns
	- SystemIndex_GthrPath: 3 columns
	- SystemIndex_PropertyStore: *598+ Columns*
		- There's a lot going on in here, but most of it will be empty depending on the file that's being indexed

[ESEDatabaseView - View/Open ESE Database Files (Jet Blue / .edb files)](https://www.nirsoft.net/utils/ese_database_view.html)


[GitHub - strozfriedberg/sidr: Search Index Database Reporter](https://github.com/strozfriedberg/sidr)
- Point to the database FOLDER, not the FILE
- Allows you to output as a CSV or JSON
- In the output, you can find many things
	- MS Edge/IE browser history
		- Will display the user SID etc.
	- File report
		- `\\` speaks to windows runtime, (winrt)
		- Full path of the item that was indexed
		- Includes additional file columns, like:
			- Date modified
			- Date Created
			- Date accessed
			- File Size
			- File Owner
			- Search Gather Time

[GitHub - kacos2000/WinEDB: Windows.EDB Browser](https://github.com/kacos2000/WinEDB)
- GUI Navigator for the EDB file


## Windows 11
Windows-gather.db
- SystemIndex_Gather
- Su
*Windows.db*
- SystemIndex_1_PropertyStory
- SystemIndex_1_PropertyStore_Metadata

## Use cases
1. Auto-summary of .msg file recovered from a computer
	1. Much faster to browse and search than the full folder structure

## Windows Search as an attack surface
- Phantom DLLs
	- SearchIndexer.exe make calls to load DLLs that don't exist
	- ...So the attacker can make those DLLs, and they get called
- SearchNightmare Windows Search Protocol Handler: ms-search
	- Opens a search window where remote malware can be execuriye by launcing a Word Doc
		- Malware can be executed 