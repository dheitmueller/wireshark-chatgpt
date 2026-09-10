# Wireshark Protocol Validation Conventions

This file records durable conventions about protocol recognition, evidence quality, portability boundaries, and packet-controlled lookups. Current upstream source remains authoritative.

## Strengthen heuristics with cheap protocol invariants

A heuristic dissector should use stable protocol invariants that are cheap to verify when those invariants materially reduce false positives. Do not stop at the minimum prefix/magic check merely because it recognizes the positive capture.

Merged MR !25483, authored by John Thacker, strengthens the HiPerConTracer heuristic after checking the implementation source and exercising large payloads: bytes beyond the minimum 16-byte header follow a predictable zero/offset pattern, and the heuristic now validates that pattern when those bytes are present. This is stronger evidence than simply matching the fixed header while remaining inexpensive on candidate traffic.

**Implementation rule:** after identifying the minimum safe recognition fields, look for additional invariant bytes/relationships that distinguish the protocol from arbitrary traffic without requiring stateful or expensive parsing. Guard every optional read by captured length and reject on invariant failure. Include negative/nonmatching traffic in validation.

**Confidence:** Very high. Merged maintainer-authored change, consistent with the notebook's existing guidance that heuristic dissectors must avoid false positives and must safely reject arbitrary traffic.

## State verification strength explicitly

When an MR adds or repairs protocol decoding, distinguish what was verified on real wire data from what was checked only against a specification, schema, generator, or reference source. Do not imply live validation for cases that were not actually observed.

Merged MR !25497 is a strong submission exemplar. Its GlusterFS decoder expansion verifies every request/reply layout field-by-field against the XDR schema and cross-checks procedure/reply bindings against both client decode and server encode source. It then separately identifies the operations observed and byte-checked in live GlusterFS captures, and explicitly labels the remaining operations as source/schema-verified only, with reasons they were not seen on the wire.

**Submission rule:** in a Testing/Verification section, name the evidence tier for each meaningful path: live capture, regression fixture, reference implementation/source cross-check, specification/schema review, or synthetic construction. State important gaps explicitly. Strong source verification is valuable, but it is not interchangeable with observing the behavior on real traffic.

**Confidence:** High. Merged master dissector change with unusually explicit evidence accounting; consistent with existing Wireshark review expectations for concrete captures and reproducible testing.

## Abstract platform semantics, not macro names

The presence of the same platform macro on multiple operating systems does not prove that the associated API has identical types, structures, or calling semantics. Prefer a semantic portability wrapper that selects the appropriate native mechanism per platform.

Merged MR !25462 fixes `ws_verify_peercred()` after code assumed that `SO_PEERCRED` implied the Linux contract everywhere. Other UN*X systems expose peer-credential facilities differently despite overlapping names. The accepted change prefers `getpeereid()` where available, uses Solaris/Illumos credential APIs where appropriate, uses Linux/Haiku `SO_PEERCRED` semantics only on the platforms that actually provide that contract, and hides those branches behind `ws_getpeereid()`.

**Implementation rule:** feature-test and reason about the API contract required by Wireshark, not only whether a preprocessor token exists. Isolate OS-specific type/layout/calling differences behind one semantic helper so callers do not accumulate platform conditionals.

**Confidence:** Very high. Merged master portability fix with broad platform intent, consistent with Wireshark's multi-OS support model.

## Use bounded semantic lookup helpers for packet-controlled indexes

Do not directly index a value/string table with a packet-controlled enum merely because valid traffic is expected to stay within the registered range. If the protocol already has a semantic lookup helper that handles unknown values, use it.

Merged MR !25506 replaces SMB2 expressions such as `smb2_cmd_vals[si->opcode].strptr` with `decode_smb2_name(si->opcode)` at multiple generated-display sites after Gerald Combs recommended the helper. Unexpected opcode values could otherwise index outside the table and trigger a global buffer overflow; the helper preserves the desired command-name behavior while safely handling the full input domain.

**Implementation rule:** when rendering or branching on a packet-derived enum/table key, search for an existing `val_to_str*`, protocol-specific decode/name helper, or other bounded lookup API before indexing the backing array directly. Treat display-only paths as security-relevant parsing paths too.

**Confidence:** Very high. Merged master memory-safety fix explicitly following Gerald Combs's recommendation.

## Bound recursive or backtracking work driven by user configuration

Configuration syntax can become an algorithmic input just as packet bytes can. A recursive wildcard/pattern matcher must not permit an otherwise valid user-supplied rule to create unbounded recursion or pathological search work.

Merged MR !24502 limits Protobuf URI-pattern recursion to 16 levels and adds a UAT update callback that rejects excessive wildcards and performs basic pattern validation before the matcher is used. The fix also removes repeated `strlen()` work from the recursive path.

**Implementation rule:** when a preference/UAT/configuration value controls recursive matching, validate structural complexity when the configuration is accepted and retain an explicit execution-time recursion/work bound as defense in depth. Do not assume configuration input is harmless merely because it is not packet-controlled.

**Confidence:** Very high. Merged master denial-of-service fix, merged by John Thacker, with validation at both configuration and execution boundaries.
