---
{"dg-publish":true,"permalink":"/tool-deep-dives/osint/exiftool/"}
---

#### exiftool
- `exiftool` is a powerful cross-platform tool for extracting [[Definitions and Topics/exif\|exif]] metadata from files and images.
	- `exiftool /path/to/file.typ`
- Files can also have their metadata changed or removed through `exiftool`
	- `exiftool -Artist="Jane Doe" /path/to/file.typ`
		- Sets the `Artist` tag to "Jane Doe"
	- `exiftool -all= /path/to/file.typ`
		- Sets all modifiable tags to blank

Sample output from a song by [WangleLine](https://wangleline.bandcamp.com/album/rabbits-journey) (you should check her out)
 ![exiftool.png](/img/user/Attachments/exiftool.png)


# Metadata

### Sources

### Tags
#tools_osint  