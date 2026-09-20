# Wireshark Wiretap Read-Cursor Conventions

This file records durable conventions for sequential and random-access cursor handling in Wiretap readers. Current upstream source remains authoritative.

## Random reads must not mutate the sequential-read cursor

Wiretap exposes sequential and random-access reading as distinct operations that may be interleaved. A random read can reposition or inspect the underlying file, but it must not update bookkeeping that represents the logical position of the sequential scan; otherwise returning to sequential reading can skip records, reread data, or calculate the next physical offset from the wrong location.

Merged master MR !15289 fixes the BLF reader after a random read in the middle of a linear scan updated `current_real_seek_pos`, a field used to track the sequential reader's physical position. The accepted change updates that value only for non-random reads, preserving the sequential cursor across intervening random lookups.

**Implementation rule:** treat sequential-read position and random-access position as separate state even if both paths share low-level decoding helpers or one file handle abstraction. Helpers invoked from both paths must receive or derive enough context to know whether advancing persistent sequential bookkeeping is appropriate.

**Review rule:** test Wiretap readers by interleaving sequential reads with `seek_read`/random reads, then resume the sequential scan and verify that the next record is exactly the one that would have followed without the random access. Sequential-only and random-only tests do not exercise this state interaction.

**Confidence:** Very high. Merged master Wiretap correctness fix with the interleaving failure mechanism stated directly in the MR.