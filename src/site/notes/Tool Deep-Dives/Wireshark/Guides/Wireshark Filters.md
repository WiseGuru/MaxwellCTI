---
{"dg-publish":true,"permalink":"/tool-deep-dives/wireshark/guides/wireshark-filters/","updated":"2024-02-14T11:55:13.000-08:00"}
---


[wireshark-filter(4)](https://www.wireshark.org/docs/man-pages/wireshark-filter.html)

## Common

| Filter type         | Display Filter (example) |
| ------------------- | ------------------------ |
| IPv4 Address        | `ip.addr==10.0.0.1`      |
| IPv4 Source         | `ip.src==10.0.0.1`       |
| IPv4 Range (Subnet) | `ip.addr==10.0.0.0/24`   |
| TCP Port            | `tcp.port==80`           |
| TCP SYNs            | `tcp.flags.syn==1`       |
| Protocol Filters    | `arp`, `udp`, etc.       |
|                     |                          |

## Wireshark Modifiers

| Modifier       | English | Shortcut                                          |
| -------------- | ------- | ------------------------------------------------- |
| Equals         | eq      | ==                                                |
| Does not Equal | not     | !                                                 |
| Or             | or      | (double bars like ll)                             |
| And            | and     | &&                                                |
| Greater than   | gt      | >                                                 |
| Less than      | lt      | <                                                 |
| Set as object  | ( )     | e.g., `!(eth.type eq 0x0800)` means NOT type 0x0800 |

**NOTE**: Using l instead of | for Obsidian formatting


## Wireshark Special Filters
| Filter                                    | Phrase                  | Example                             |
| ----------------------------------------- | ----------------------- | ----------------------------------- |
| Contains an EXACT string (case sensitive) | contains "exact string" | frame contains "Google"             |
| Matches a Regular Expression (RegEx)      | matches "regex"       | http.host matches "\.(orglcomlnet)" |
| Anything in a range                       | in {range}              | tcp.port in {80 443 8000..8004}     |

**NOTE**: Using l instead of | for Obsidian formatting


No Chatter filter:
`!(eth.dst== ff:ff:ff:ff:ff:ff or arp or lldp or cdp or stp)`


