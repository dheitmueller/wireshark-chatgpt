# Wireshark Wiretap Read-Cursor Conventions

This file records durable conventions for sequential and random-access cursor handling in Wiretap readers. Current upstream source remains authoritative.

## Random reads must not mutate the sequential-read cursor

Wiretap exposes sequential and random-access reading as distinct operations that may be interleaved. A random read can reposition or inspect the underlying file, but it must not update bookkeeping that represents the logical position of the sequential scan; otherwise returning to sequential reading can skip records, reread data, or calculate the next physical offset from the wrong location.

Merged master MR !15289 fixes the BLF reader after a random read in the middle of a linear scan updated `current_real_seek_pos`, a field used to track the sequential reader's physical position. The accepted change updates that value only for non-random reads, preserving the sequential cursor across intervening random lookups.

**Implementation rule:** treat sequential-read position and random-access position as separate state even if both paths share low-level decoding helpers or one file handle abstraction. Helpers invoked from both paths must receive or derive enough context to know whether advancing persistent sequential bookkeeping is appropriate.

**Review rule:** test Wiretap readers by interleaving sequential reads with `seek_read`/random reads, then resume the sequential scan and verify that the next record is exactly the one that would have followed without the random access. Sequential-only and random-only tests do not exercise this state interaction.

**Confidence:** Very high. Merged master Wiretap correctness fix with the interleaving failure mechanism stated directly in the MR.

## Make the sequential path genuinely forward-only when the format can be streamed

A Wiretap reader that supports sequential input should not depend on seeking merely because the on-disk format also supports random access. Pipes and other non-seekable inputs can only move forward, so state needed during linear decoding must be accumulated as records are consumed rather than reconstructed by seeking backward or jumping around the file.

Merged master MR !13399, authored by Giovanni Musto and merged by Anders Broman, restructures the BLF reader to prepare for sequential log-container reading, implements a linear-read mechanism, and then enables a pipe as input. The accepted series separates the forward scan needed to discover/decode successive BLF objects from assumptions that the input is a seekable regular file.

**Architecture rule:** when a capture format is streamable, make the sequential `read` path valid using forward consumption alone. Keep seek-dependent behavior confined to random-access operations and to formats whose semantics genuinely require seekable input.

**State rule:** preserve container/object context that is learned during the forward scan in reader state when later records need it. Do not make the sequential parser seek backward merely to rediscover information it has already passed.

**Testing rule:** exercise the same representative capture both as a regular file and through a non-seekable pipe/stdin path, including transitions across container boundaries. Where the format also supports random access, retain separate `seek_read` tests so streamability is not achieved by silently breaking indexed/random reads.

**Confidence:** High. Merged master architectural series whose explicit endpoint was allowing BLF input from a pipe, with the linear-reader preparation and implementation carried in the same accepted MR.