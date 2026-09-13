# Wireshark Parser State-Boundary Conventions

This file records durable rules for keeping wire-format parsing independent from optional semantic/state bookkeeping. Current upstream source remains authoritative.

## Optional state must not suppress mandatory wire consumption

When the wire format says a field is present, parse it and advance the offset regardless of whether optional state needed for later correlation or presentation is available. Conditional state recording belongs *after* or around the storage step, not around the wire read itself.

Merged master MR !21641, authored by John Thacker and approved/merged by Gerald Combs, fixes SMB2 FID parsing where `dissect_nt_guid_hnd()` had been guarded by `if (si->saved)`. When saved filename/state was absent, the GUID was neither dissected nor consumed, leaving the parser offset wrong for subsequent fields. The accepted fix always dissects the GUID and advances the offset, while only assigning the returned `hnd_item` into `si->saved` when that state object exists.

**Parser rule:** separate mandatory packet consumption from optional bookkeeping. A missing cache/conversation/UI/state object may prevent storing a decoded result, but it must not change which bytes the protocol says are present or where the next field begins.

**Confidence:** Very high. Merged John Thacker correctness fix with the failure mode stated directly in the MR.

## Terminate constructed strings at the amount actually produced, not an inconsistent packet-declared length

If malformed input makes two parts of a parser disagree about a packet-declared string length, the NUL terminator must follow the bytes actually written into the destination. Using the untrusted/declarative length to place the terminator can leave the constructed C string inconsistent with the copy operation and can create out-of-bounds or stale-data behavior.

Merged DOF fix !21651, authored and merged by John Thacker on release-4.6, explicitly changes the interface-ID string path to terminate after the last character written. The MR also notes that the surrounding manual string handling would be better expressed with `wmem_strbuf`, which avoids hand-maintained capacity/termination bookkeeping.

**Parser rule:** when producing a string from malformed or normalized packet input, track destination bytes produced independently from source/declarative length and terminate at the produced length. Prefer Wireshark string/buffer abstractions when they remove manual size and termination state.

**Confidence:** High. Merged stable-branch safety fix authored by John Thacker; repeated cherry-picks of the same underlying change are corroboration, not independent evidence.