# Wireshark TVBuff Invariant Notes

Current upstream source remains authoritative.

## Captured length cannot exceed reported length

Merged master MR !8005, authored by Guy Harris, makes the core TVBuff relationship explicit: a packet cannot have more captured bytes than its logical reported length. The Frame dissector reports the inconsistent input and repairs the TVBuff before downstream dissection. Release-4.0 MR !8007 carries the same change.

Review rule: distinguish this impossible relationship from normal capture truncation. Downstream parsing should see a consistent TVBuff length model.

## Normalize search coordinates once

Merged master MR !8003, authored by John Thacker, fixes tvb_find_guint16 after an earlier partial match could break its searched-length accounting. The accepted implementation resolves negative offsets and an open-ended maximum into an absolute start and bounded limit before entering the loop.

Implementation rule: convert caller-facing coordinate conventions once at API entry, then keep loop progress and boundary checks in one normalized coordinate system.
