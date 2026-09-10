# Wireshark Source Layout Conventions

This file records durable source-tree naming and responsibility conventions extracted from accepted upstream changes. Current upstream layout remains authoritative.

## Name a source file for the responsibility it actually implements

Do not classify a file as generated/static `data-*` merely because much of its content is generated tables or protocol data if the file still contains live dissector implementation. File naming should reflect the source unit's real architectural responsibility; split responsibilities first if a different classification is desired.

Merged MR !25026, authored by John Thacker and merged by Michael Mann, restores `packet-netlink-nl80211.c` after a broader rename had moved several generated-data files to `data-*`. The nl80211 file was different: it still contained actual dissection logic. The MR explicitly notes that splitting its data into a separate file might make sense, but that split had not yet occurred.

**Implementation rule:** keep files containing dissector implementation in the `packet-*` class. If generated/data-only material should become `data-*`, first isolate it so the resulting source file is actually data-only rather than changing the name while retaining mixed responsibilities.

**Confidence:** Very high. Merged master correction authored by John Thacker and merged by Michael Mann.

## Place shared sources and headers at the lowest layer that actually owns them

Source-tree placement is part of the dependency graph. A header should not live in a lower-level development package if including it necessarily drags in higher-level library dependencies, and implementation used only by one subsystem should not remain in a generic/common target merely because it historically lived there.

Merged MR !24552, authored and merged by John Thacker, moves `cfile.h` under `epan/libwireshark` because its interface depends on epan, wiretap, and wsutil and is consumed by libuiqt-plugin. Keeping it at the old lower-level location made the installed-header dependency graph misleading and could force lower development packages to depend upward. Merged MR !24556, also authored and merged by John Thacker, moves sync-pipe write implementation into the capture library because only the capture child path needs it, reducing unnecessary linkage and preserving a directed dependency graph.

**Implementation rule:** choose source/header ownership from the narrowest architectural layer that owns both the API semantics and its dependencies. When moving code, inspect the resulting build/install dependency DAG; do not leave a header or implementation in a nominally generic layer if its real dependencies point upward or its only consumers live in a more specific subsystem.

**Confidence:** Very high. Two merged master refactors authored and merged by John Thacker, both explicitly motivated by dependency layering.