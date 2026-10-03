# Crypto Context Lifecycle Conventions

This file records durable ownership and reuse guidance for cryptographic library contexts. Current dependency documentation and upstream Wireshark source remain authoritative.

## Reuse resettable contexts across loops instead of repeated destroy/recreate cycles

When a cryptographic library explicitly supports resetting a context for a new independent operation, the surrounding loop should normally own one context: initialize or open it before the loop, reset and rekey or reseed as required for each iteration, and clean it up once after the loop.

Merged MRs !1047, !1049, !1052, and !1054 apply that pattern to libgcrypt HMAC, SHA/MD5, generic digest, and AES cipher handles. Besides avoiding repeated allocation/destruction overhead, the changes remove lifecycle shapes that Coverity reported as possible double frees.

**Implementation rule:** use a reset operation only after confirming its documented post-reset state is sufficient for the next operation. Reapply keys or other state that reset clears. Keep a single clear owner responsible for final cleanup.

**Review rule:** repeated open/init followed by close/cleanup inside a loop is worth checking for a reset/reuse API, especially when static analysis reports double-free or lifetime ambiguity. Do not silence the warning by weakening ownership checks; simplify the lifecycle when the library contract permits it.

**Confidence:** High. Four independent merged changes apply the same libgcrypt reuse pattern, including review or approval by Anders Broman and Ronnie Sahlberg.
