# Wireshark Optional-Value Validity Conventions

This file records durable conventions for optional values whose storage is meaningful only when a separate validity or presence indicator says it is initialized. Current upstream source remains authoritative.

## A validity flag must guard the read itself

When a structure uses a presence/validity flag for an optional member, do not read the member when the flag is false merely because a later branch, fallback, or display path appears not to use the value. The storage may be uninitialized, and accessing it can itself be undefined behavior.

Merged master MR !14985, authored and merged by John Thacker, fixes `wlan-radio` by avoiding access to `phdr.phy_info.info_11b.short_preamble` when `has_short_preamble` is false. The optional value is not always initialized in that case; if an assumed value is later needed, the accepted path assigns it before use rather than first reading invalid storage.

**Implementation rule:** treat the validity/presence predicate as part of the value's access contract. Test the predicate before any load of the optional member, and initialize a fallback explicitly before using it. Do not rely on zeroed storage, incidental initialization on another path, or the fact that the loaded value will later be ignored.

**Review rule:** whenever a field has a companion `has_*`, `valid`, `present`, or equivalent flag, inspect all reads of the value, not just presentation logic. A false validity flag can mean that the backing bytes are indeterminate rather than simply semantically unknown.

**Confidence:** Very high. Merged master undefined-behavior fix authored and merged by John Thacker with the initialization contract stated directly in the MR.
