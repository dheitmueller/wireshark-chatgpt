# Reassembly conventions from MRs 359-408

Merged master MR !403, authored by John Thacker and reviewed by Peter Wu, fixes offset-based reassembly so duplicate and wholly contained fragments are still classified as overlaps even when they add no new bytes. Matching retransmissions set the overlap flag; differing bytes additionally set the overlap-conflict flag. The change also validates offset-plus-length arithmetic before using it and checks the current fragment's data object before access.

Peter Wu explicitly requested a regression test. John added unit tests for partial reassembly, duplicate middle fragments, conflicting duplicates, and the existing completed-reassembly edge behavior.

**Rule:** overlap classification is determined by byte-range intersection and content, not by whether a fragment extends the assembled end. Validate the exact fragment and its range before comparing or copying data.

**Testing:** cover exact duplicates, conflicting duplicates, contained overlaps, and extending fragments.
