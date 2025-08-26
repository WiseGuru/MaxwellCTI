---
{"dg-publish":true,"permalink":"/definitions-and-topics/vad/","updated":"2024-03-12T16:21:44.000-07:00"}
---

#### VAD
- "The Virtual Address Descriptor, commonly known as the "VAD", is a fundamental component in the memory management system of the Windows operating system. Primarily it is responsible for managing and tracking the memory allocations within a process's virtual address space."^[[Understanding the VAD tree](https://www.linkedin.com/pulse/understanding-virtual-address-descriptor-vad-windows-memory-taz-wake-etoue/)]
- TBH, still a little little over my head, but the key here is that there are certain expected VAD values, and specific tags can be an indication of compromise, and is used by [[Tool Deep-Dives/Volatility\|Volatility]]'s `malfind` to identify code and DLLs.
	- e.g., Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
		- Memory sections should be designated as either "READ" or "WRITE", not both
		- By using dynamic memory spaces, it can make finding static analysis more difficult because the code can change.




# Metadata

### Sources
[BSODTutorials: Virtual Address Descriptors (VADs) - !vad](https://bsodtutorials.blogspot.com/2013/11/virtual-address-descriptors-vads-vad.html)
[The VAD tree: A process-eye view of physical memory - ScienceDirect](https://www.sciencedirect.com/science/article/pii/S1742287607000503)
[Understanding the VAD tree](https://www.linkedin.com/pulse/understanding-virtual-address-descriptor-vad-windows-memory-taz-wake-etoue/)
[APT Memory and Malware Challenge Solution | SANS Institute](https://www.sans.org/blog/apt-memory-and-malware-challenge-solution/)
### Tags
#defs_sec 