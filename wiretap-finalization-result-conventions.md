# Wireshark Wiretap Finalization-Result Conventions

## Return state from the lifecycle operation that makes it final

Merged master MR !6432, authored by Guy Harris, changes the Wiretap close API so it can return an optional reload indication. ERF can establish that result during close; reading it before close can be stale, while reading it after close is impossible because the dumper handle is no longer valid.

Closed MR !6430 proposed setting the result earlier during open. Guy rejected that workaround in favor of reshaping the API, then implemented !6432. Merged stable MRs !6435 and !6438 preserve the semantics through a temporary compatibility entry point where changing the stable ABI directly was inappropriate.

**Rule:** lifecycle-derived state belongs on the operation that makes the state authoritative. Do not predict it earlier merely to preserve an inadequate getter, and do not require callers to inspect an object after the operation that destroys it.

**Stable-branch rule:** master may evolve an API directly while a maintained dot release can use a narrowly scoped compatibility wrapper to carry the semantic fix without breaking its ABI.

**Confidence:** Extremely high. The master design is authored by Guy Harris, corroborated by his direct rejection of the superseded workaround and by two merged stable backports.
