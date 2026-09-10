# Wireshark Source Layout Conventions

This file records durable source-tree naming and responsibility conventions extracted from accepted upstream changes. Current upstream layout remains authoritative.

## Name a source file for the responsibility it actually implements

Do not classify a file as generated/static `data-*` merely because much of its content is generated tables or protocol data if the file still contains live dissector implementation. File naming should reflect the source unit's real architectural responsibility; split responsibilities first if a different classification is desired.

Merged MR !25026, authored by John Thacker and merged by Michael Mann, restores `packet-netlink-nl80211.c` after a broader rename had moved several generated-data files to `data-*`. The nl80211 file was different: it still contained actual dissection logic. The MR explicitly notes that splitting its data into a separate file might make sense, but that split had not yet occurred.

**Implementation rule:** keep files containing dissector implementation in the `packet-*` class. If generated/data-only material should become `data-*`, first isolate it so the resulting source file is actually data-only rather than changing the name while retaining mixed responsibilities.

**Confidence:** Very high. Merged master correction authored by John Thacker and merged by Michael Mann.