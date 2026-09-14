# Wireshark Registration Side-Effect Conventions

This file records durable conventions for registration helpers whose calls update more than one registry or namespace. Current upstream source remains authoritative.

## Register each logical object once; understand helper side effects

Before manually registering metadata adjacent to a protocol/dissector registration, check whether the higher-level helper already performs that registration as a side effect. Re-registering the same logical object is at best redundant and can produce inconsistent names, wasted startup work, or lifecycle bugs when one path upgrades/replaces an earlier registration.

Merged master MR !20638, authored and merged by John Thacker, removes a second Bluetooth preferences-module registration. The Bluetooth subtree had already been upgraded to a full preferences module; the duplicate-registration guard did not catch that upgrade path, so calling `prefs_register_protocol()` again was still wrong.

Merged master MRs !20613 and !20615, also authored and merged by John Thacker, remove duplicate OID name registration where `register_ber_oid_dissector()` already calls `oid_add_from_string()`. !20615 also preserves the name that was effectively visible after the old duplicate registrations when the two registrations used different names, avoiding an accidental presentation change while removing the redundant work.

**Implementation rule:** treat registration helpers as APIs with side effects, not merely constructors returning a handle. Identify every registry they update before adding a second explicit registration for preferences, OID names, Decode-As metadata, or similar global registration state.

**Review rule:** when two registration calls refer to the same protocol/OID/logical object, determine whether they are intentionally registering distinct semantics or are duplicating a side effect. During deduplication, preserve the effective user-visible name/behavior unless a semantic change is intentional and documented.

## Prefer structured OID constants over string parsing when the OID is compile-time data

For fixed OIDs known at compile time, prefer the registration API that consumes a static integer-array representation when practical instead of reparsing dotted-decimal strings at startup.

Merged master MR !20604, authored by John Thacker and merged by Anders Broman, fixes several OID registrations where string-API arguments had been swapped or adjacent string literals had accidentally concatenated. The MR explicitly notes that the static integer-array form both avoids this class of argument/string error and avoids repeatedly converting a constant string into integers at runtime.

**Implementation rule:** represent compile-time protocol identifiers in the most structured form accepted by the API. String-based registration is appropriate when the input is genuinely textual/dynamic; do not choose it by default for constants when a typed representation is available.

**Confidence:** High to very high. The duplicate-registration rule is independently demonstrated by three merged master MRs from John Thacker; the structured-OID guidance comes from a merged master corrective change with a clearly stated safety/performance rationale.
