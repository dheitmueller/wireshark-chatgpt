# Wireshark Dissector Resilience Conventions

This file records durable conventions for preserving useful dissection output when captures are malformed, truncated, or otherwise incomplete. Current upstream source remains authoritative.

## Publish successfully decoded summary information incrementally

When a protocol-tree item's summary is assembled from multiple packet fields, append each trustworthy piece of summary text as soon as the corresponding field has been successfully fetched. Do not defer all summary construction until after later reads that can throw a bounds exception.

Merged MR !20958, authored and merged by Guy Harris, changes the DAAP TLV dissector accordingly. The dissector now appends the decoded tag name immediately after fetching it and appends the size immediately after fetching that field. If a later field lies beyond captured data and TVB access throws, users still see the information that was successfully decoded before the truncation.

**Implementation rule:** treat already-decoded protocol-tree information as durable output. Build item labels/summaries in parser order as fields become valid, rather than making presentation of earlier valid fields contingent on later packet accesses succeeding.

**Review rule:** for code that creates a subtree and later enriches its label, inspect every intervening TVB access or subdissector call that can throw. If useful label information is already known before that point, publish it before the potentially failing operation.

**Confidence:** Extremely high. Merged master change authored and merged by Guy Harris with the truncation behavior explicitly stated as the reason for the change.

## Malformed packet content should normally be reported, not asserted

Assertions in dissector paths should protect genuine programmer/internal invariants, not assumptions that untrusted packet bytes are well formed. When packet content can violate a protocol constraint, prefer normal malformed/expert reporting and terminate or bound that parsing path safely.

Merged MR !20951, authored and merged by Martin Mathieson, replaces a PIM dissector assertion reached by malformed input with expert information. The change was made while investigating a fuzz failure and was accepted independently of whether it was the exact triggering defect.

**Implementation rule:** before asserting on a value derived from packet data, ask whether a malformed or truncated packet can produce it. If so, emit appropriate expert information and handle the condition as input error instead of terminating the dissector process.

**Confidence:** Very high. Merged master change by a senior dissector maintainer and direct fuzzing context; it also corroborates the notebook's broader assertion/static-analysis guidance.

## When the build cannot represent a decoded value accurately, say so instead of fabricating precision

A dissector may be portable to a platform that lacks an arithmetic capability needed for one optional high-precision calculation. In that case, keep the protocol dissection usable, but do not substitute a plausible-looking lower-precision or incorrect value. Mark the affected field as undecoded/unsupported with Expert Info so users can distinguish a platform capability limit from actual packet data.

Merged MR !20646, authored and merged by Guy Harris, handles LDANeo picosecond timestamps this way on platforms where the required 128-bit-divisor operation is unavailable. Rather than displaying nanosecond/picosecond fields that cannot be computed correctly, the dissector adds an `undecoded` expert warning in their place.

**Implementation rule:** optional numeric decoding should degrade explicitly. If an exact result cannot be computed on a supported build target, omit that derived value and report the limitation; never present an approximation as though it were the protocol value unless the protocol/API explicitly defines that approximation.

**Confidence:** Extremely high. Merged master change authored and merged by Guy Harris, with the graceful-degradation behavior stated directly in the MR description.

## Bound child TVBs to captured data when a declared length exceeds what is available

A wire-declared subobject length can be semantically valid yet exceed the bytes present in a truncated capture. When useful partial decoding is possible, construct the child view from the captured remainder, report the declared-vs-available mismatch with Expert Info, and carry the bounded effective length into downstream parsing.

Merged master MR !1669 updates PER open-type dissection accordingly. Before creating the octet-aligned child TVB, it calculates the captured bytes available from the open-type offset. If that amount is shorter than the declared PDU length, the accepted implementation creates the child from the captured amount, adds an expert error, and replaces the downstream length with that bounded amount rather than asking later code to operate on the unavailable declared size.

**Resilience rule:** distinguish declared length from available/effective length after truncation has been detected. Once the parser chooses partial decoding, all child views and subsequent bounds must use the effective captured length while the expert item preserves the original protocol discrepancy for the user.

**Confidence:** High. Merged master core PER change authored and merged by Anders Broman.
