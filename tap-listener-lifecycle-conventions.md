# Wireshark Tap Listener Lifecycle Conventions

This file records durable ownership and cleanup conventions for tap listeners and their private state. Current upstream tap APIs remain authoritative.

## Give listener-owned private allocations an explicit finish path

If registering a tap listener allocates private state that is intended to live for the listener lifetime, pair that allocation with the listener's supported finish/cleanup callback rather than relying on process exit or an unrelated caller to reclaim it.

Merged master MR !1686 fixes a leak in TShark simple-statistics taps. The initialization path allocates table-private data for the tap listener, but the registration previously supplied no finish callback. The accepted change registers `simple_finish()`, which releases the listener-owned allocation when the tap is finished. The leak was demonstrated with LeakSanitizer using `tshark -r <capture> -z dhcp,stat -q`.

**Ownership rule:** when listener registration transfers or retains an allocation for the listener's lifetime, make the corresponding destruction path explicit in the same registration contract. Do not assume short-lived CLI execution makes the leak harmless.

**Testing rule:** lifecycle bugs in statistics/tap code are good sanitizer targets because setup and teardown are deterministic. Exercise a complete register/use/finish cycle under ASan/LeakSanitizer, not only packet processing.

The MR also shows that project API-check scripts can flag identifiers as well as calls: `checkAPIs.pl` treated a local variable named `stat` as a prohibited API occurrence, so the contributor renamed it to `stat_data`. Checker diagnostics should be understood in context, but code submitted upstream still needs to satisfy the repository's supported checker behavior.

**Confidence:** High. Merged master leak fix with an explicit sanitizer reproducer and lifecycle correction.
