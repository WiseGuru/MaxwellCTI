---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/guides/geo-ip-databases/"}
---

## How to add GeoIP information to Wireshark
1. Download and extract the databases from MaxMind (great name)
	1. Have to sign up for a free account and GeoLite 2 license
	2. Navigate to Downloads, and download the ASN, City, and Country databases in GZIP (not the CSV format versions)
		1. I have them saved/extracted to `C:\Users\whyno\Documents\Wireshark\MaxMind GeoIP Databases`
	3. You have to extract them not just from the GZ, but also from the TAR
2. Activate them in Wireshark
	1. Edit>Preferences>Name Resolution>MaxMind database directories