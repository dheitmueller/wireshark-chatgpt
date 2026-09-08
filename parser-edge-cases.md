# Wireshark Parser Edge-Case Conventions

This file records durable parser-correctness conventions extracted from upstream Wireshark merge requests. Current upstream source remains authoritative; these notes are a curated review cache.

## Initialize optional parser outputs before calling decoders that may leave them unset

When a decoder fills an output pointer only when a value is present, initialize that output to a safe sentinel before the call and test it before use. Do not assume that successful return implies every optional output was assigned.

Merged MR !26038, authored and merged by John Thacker and approved by Anders Broman, fixes ESS generated ASN.1 handling of zero-length BIT STRINGs. `dissect_ber_bitstring()` can leave the `VAL_PTR` tvbuff unset without throwing, so the generated callback now initializes `attributes` to `NULL` and calls `ess_dissect_attribute_flags()` only when a tvbuff was actually produced. The same change is preserved in release backports !26049 and !26050.

**Implementation rule:** initialize out-parameters according to their "no result" contract before invoking parsers that can legitimately produce no object, then guard every downstream dereference or helper call.

**Confidence:** Very high. Merged master fix by John Thacker, maintainer approval, and two accepted backports.

## Nested or re-entrant decoding can invalidate outer transient state

Parser scratch state shared through globals, generated-code support variables, or other non-stack storage must be assumed vulnerable to nested decoding. A malformed inner structure can reset or overwrite the same state that an outer invocation still expects.

Merged MR !26046, authored by Peter Wu, approved and merged by John Thacker, fixes X.509 IF dissection where a malformed RDN nested inside another RDN re-entered `dissect_x509if_RelativeDistinguishedName()`. The inner call reset `last_rdn_buf` to `NULL`; the outer call then dereferenced it. The accepted fix treats the scratch result as optional after the nested decode and skips the formatting/aggregation path when it was cleared. Release backports !26047 and !26048 preserve the same behavior.

**Implementation rule:** when recursive or nested parsing can touch the same transient state, do not assume an outer invocation still owns a valid value after the inner call returns. Prefer packet/local state where practical; otherwise revalidate the state before use.

**Confidence:** Very high. Security/correctness master fix with explicit nested-call failure mechanism, John Thacker approval, and two release backports.

## Autodetection should use structural version invariants; forced versions should report violations

When multiple protocol versions share framing, structural constraints that distinguish versions belong in autodetection. If the user explicitly selects a version, however, do not silently reinterpret malformed traffic as another version merely to avoid a warning.

Merged MR !26051, authored by John Thacker and approved/merged by Anders Broman, uses the STUN message-length multiple-of-four requirement introduced by RFC 5389 as a version heuristic. In automatic mode, a non-multiple-of-four length selects the older RFC 3489 interpretation. With an explicit RFC 5389-or-newer selection, the dissector retains that requested interpretation and attaches expert information to the length field instead.

**Implementation rule:** separate "which version is this?" evidence from "is this valid for the selected version?" diagnostics. Autodetection may use version-defining invariants; explicit user configuration should generally be honored while malformed input is reported as malformed.

**Confidence:** Very high. Merged master change authored by John Thacker with maintainer approval.

## Preserve the full return-value contract; boolean literals are unsafe in tri-state APIs

Before returning `true`, `false`, `0`, or `-1` from a helper, verify the callee/caller contract. Many wiretap and parser routines use integer return values where negative means error, zero means a non-error condition such as EOF/no packet, and positive means success. Returning `false` from an error path can therefore convert a hard failure into apparent success or EOF.

Merged MR !26054, authored by Ronnie Sahlberg and approved/merged by John Thacker, changes two malformed-packet paths in `peektagged_read_packet()` from `return false` to `return -1`. Those paths had already set `WTAP_ERR_BAD_FILE`, but returning zero caused callers to believe no error had occurred and that packet-buffer state could be consumed incorrectly.

**Implementation rule:** treat return type and return semantics separately. An `int` API that happens to be tested in boolean contexts may still have a multi-valued contract; use its named/required error value rather than a boolean synonym.

**Confidence:** Very high. Merged wiretap correctness fix with John Thacker approval.

## Represent self-delimiting payloads as subset tvbuffs instead of carrying outer offsets plus lengths

When a PDU header declares a payload length, prefer constructing a subset tvbuff for that payload and passing the subset into payload decoders. This makes the declared protocol boundary an actual tvbuff boundary and avoids repeated `header + payload` arithmetic that can overflow or be applied inconsistently.

Merged MR !26055, authored, approved, and merged by John Thacker, changes AgentX PDU dissection so payload routines receive a subset tvbuff and use offsets relative to zero. Loop termination then uses `tvb_reported_length_remaining()` rather than repeatedly comparing offsets against a separately computed end. This independently reinforces the same design already captured from !25978.

**Implementation rule:** when a protocol supplies an explicit substructure length, encode that length in the tvbuff hierarchy whenever practical. Let tvbuff bounds enforcement replace hand-maintained end-offset arithmetic.

**Confidence:** Very high. Merged OSS-Fuzz hardening authored and merged by John Thacker; independently corroborates an existing notebook rule.

## Lazily create aggregate tvbuff containers only when a real member will be attached

An aggregate/composite parser object can have ownership or finalization requirements even when no payload bytes ultimately get attached to it. If the append operation intentionally ignores zero-captured-length members, creating the aggregate before proving a non-empty member exists can leak or leave an invalid empty aggregate.

Merged MR !26056, authored, approved, and merged by John Thacker, fixes CBOR indefinite-string handling where `tvb_new_composite()` could be allocated even though `tvb_composite_append()` would skip the only member because its captured length was zero. The accepted code creates the composite only when the chunk has both a logical data length and captured bytes remaining.

**Implementation rule:** align container creation with the append API's acceptance criteria. If an append can be a no-op for empty/truncated input, delay allocation until a member that will actually be retained is known to exist.

**Confidence:** Very high. Merged OSS-Fuzz leak fix authored and merged by John Thacker.

## Stateful conversation identity may be protocol-semantic rather than transport-specific

When protocol state is intentionally shared across transports, a normal transport conversation tuple can be too narrow. Build the lookup key from the stable semantic endpoints and protocol identifier that define the state, rather than from TCP/UDP stream identity merely because the current packet arrived on that transport.

Merged MR !26045, authored by Jaap Keuter and committed/merged by John Thacker, fixes Synchrophasor configuration sharing where TCP can deliver configuration later consumed by UDP packets. The accepted implementation adds a Synchrophasor conversation type and keys it by source/destination addresses plus the protocol ID, allowing the configuration to be found across transport boundaries.

**Implementation rule:** choose conversation keys from the lifetime and scope of the state being modeled. If the specification permits one transport to configure another, transport ports/stream IDs must not artificially partition that state.

**Confidence:** High. Merged master architecture change, with John Thacker as committer/merger and a concrete cross-transport requirement.