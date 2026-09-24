# Wireshark Runtime Configuration-State Conventions

This file records durable conventions for configuration changes that interact with already-running operations and derived application state. Current upstream source remains authoritative.

## Use realized operation state after a mutable preference has been applied

A preference is configuration input, not necessarily the authoritative state of an operation already in progress. If a live operation allocates resources or chooses behavior from a preference at startup, later changes to that preference must not make the running operation pretend those resources were or were not created.

Merged MR !10982 changes live-capture duplicate-frame handling so the checksum/cache are allocated when capture starts and the presence of `frame_cksum` becomes the authoritative marker for whether deduplication is active. The MR explicitly notes that the user can change the preference during a live capture, so packet processing and cleanup cannot safely keep consulting `prefs.ignore_dup_frames`. Cleanup likewise tests actual allocation state instead of the current preference value.

**Implementation rule:** at an operation boundary, realize mutable configuration into operation-owned state. During the operation, base behavior and teardown on that realized state and actual ownership, not on rereading a preference that may have changed since startup.

**Confidence:** Very high. Merged master lifecycle fix whose stated rationale is the preference/runtime-state mismatch.

## Treat configuration effect flags as independent invalidation domains

Different configuration changes can invalidate different layers of derived state. A field-registry change does not inherently imply packet redissection, and redissection does not follow merely because another UI cache must be refreshed.

Merged MR !10978, with the equivalent merged !10977 backport, corrects the documented semantics of `UAT_AFFECTS_FIELDS`: it queues `FieldsChanged` but does not itself trigger `PacketDissectionChanged`. Display-filter macros are the important exception where the named-field environment must be rebuilt without redissecting packets, so their UAT correctly uses only `UAT_AFFECTS_FIELDS`.

**Implementation rule:** model configuration effects with the smallest accurate set of independent invalidation flags. Do not rely on undocumented implication between flags or request a stronger refresh merely because most existing callers happen to request both.

**Confidence:** Very high. Merged John Thacker fix and stable counterpart, with the exact distinction documented in the UAT flag comments.
