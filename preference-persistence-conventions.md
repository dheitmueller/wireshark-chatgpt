# Wireshark Preference Persistence Conventions

This file records durable conventions for saving preference state and distinguishing an explicit reset from the absence of an update. Current upstream source remains authoritative.

## Sparse preference serialization must represent an explicit empty value

A persistence layer that serializes only non-default entries still needs a way to say that a previously non-empty preference should become empty. Omitting a key cannot serve both as "leave the old value alone" and "replace the old value with the default"; those are different state transitions.

Merged master MR !15836, authored by John Thacker and approved/merged by Anders Broman, fixes the Qt interface preference cache for interface comments and monitor-mode settings. The save path only appended entries for interfaces whose values were non-default. If every interface was reset to the default, the preference key disappeared from the temporary map entirely, so the old persisted non-empty value was never overwritten. The accepted fix pre-seeds those preference entries with empty `QStringList` values, allowing the subsequent writeback to serialize an explicit empty preference. The same correction was accepted on the maintained 4.2 and 4.0 branches in !15840 and !15841.

**Implementation rule:** when a sparse map/list is used to build persistent preference updates, distinguish "no update for this preference" from "update this preference to empty/default." If clearing all members is a valid user action, make sure the serialization path emits an explicit empty/default value rather than dropping the key because there are no non-default elements.

**Review rule:** test the transition from some non-default entries to zero non-default entries. A save path that works when at least one item remains customized can still fail when the last customized item is cleared.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker and approved by Anders Broman, independently carried to both maintained release branches.
