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

## Parse decimal timestamp fractions as decimal fractions, not fixed-unit integers

A textual fractional-seconds field carries its scale in the number of decimal digits. Parsing the digits directly as an integer and multiplying by a fixed factor is only correct when the input width is fixed. When the format permits fewer digits, normalize the decimal representation to the target precision before numeric conversion.

Merged MR !26061 fixes the Daintree SNA wiretap reader by accepting at most nine decimal digits with `%9[0-9]`, padding the captured fractional string with trailing zeroes to nine digits, and then converting it to nanoseconds. This both validates the character domain and preserves the meaning of values such as `.1`, `.01`, and `.001`.

**Implementation rule:** for variable-width decimal fractions, validate a bounded digit string and scale by decimal position (for example by right-padding to nanosecond precision) rather than assuming the parsed integer already has a fixed unit.

**Confidence:** High. Merged master wiretap fix approved/merged by Anders Broman.

## Bounds-check every dimension before indexing packet-derived state arrays

When packet-derived state selects indices into a multidimensional fixed array, validate each index against the concrete backing dimension before the read or write. Do not rely on earlier protocol-level validation, enum ranges, or assumptions about direction values when corruption of parser state could make an index escape the storage.

Merged MR !26072 fixes potential heap corruption in UMTS RRC by checking both `rbid` against `G_N_ELEMENTS(ciphering_info->seq_no)` and `direction` against `G_N_ELEMENTS(ciphering_info->seq_no[0])` before writing `ciphering_info->seq_no[rbid][direction]`; an invalid value returns without performing the write.

**Implementation rule:** guard the actual storage expression immediately before indexing it, and derive bounds from the array itself where practical (`G_N_ELEMENTS`) so the check remains coupled to the allocation.

**Confidence:** High. Merged master memory-safety fix authored and merged by Ronnie Sahlberg.

## Explicit-length byte buffers are not C strings

When an external API returns a pointer plus an explicit byte count, that count is the complete access contract. Text-looking content in the buffer does not make it NUL-terminated, does not establish a minimum width, and does not make string-scanning functions safe.

Merged MR !26099, authored by Ronnie Sahlberg and merged by Anders Broman, fixes LDAP ETW parsing in `etwdump`. The ETW property allocation was exactly `TdhGetPropertySize()` bytes, yet the old code used prefix/string parsing and fixed hexdump offsets as though the property were terminated and at least one full line long. Short but valid final lines could read beyond the allocation, and the in-place decoder could also write beyond it. The accepted fix checks `Length` before every fixed-width access, uses bounded `memcmp()` for prefixes, converts hex digits explicitly with `g_ascii_xdigit_value()`, decodes into a separately sized local output buffer, and rejects the zero-length allocation case.

**Implementation rule:** for pointer-plus-length input, prove `length >= needed` before every fixed-width read; use length-aware comparison/parsing primitives; and do not scan beyond the supplied count looking for a terminator that the producer never promised. If decoding changes representation or density, prefer a separately bounded output buffer unless in-place capacity and overlap semantics are explicitly proven.

**Confidence:** Very high. Merged master memory-safety fix from an experienced maintainer, accepted by Anders Broman, with the failure mechanism and valid-short-input case documented in the MR.

## Enforce schema multiplicity and output bounds structurally, and keep cleanup on error paths

Structured-input parsers should not rely on a well-formed file to prevent repeated writes into one logical output slot. If the format permits exactly one child/value, stop consuming that slot after the first accepted instance (or reject a duplicate). Likewise, an output cursor check should reject values that are already beyond the end, not only the exact end position.

Merged MR !26105, authored and merged by John Thacker, hardens the Gammu DCT3 trace wiretap reader. A valid `<l1>` has only one `<l2>` child; the accepted parser now stops after handling that child rather than allowing another matching element to reuse the same output cursor. Its `hex2bin()` guard changes from `out == out_end` to `out >= out_end`, making the write boundary robust even if earlier state has already advanced past the nominal end. The same patch also changes a direct error return to the common cleanup path so the XML document is freed. Release-4.6 backport !26107 preserves the behavior.

**Implementation rule:** encode one-of/one-child schema constraints in control flow close to the write; use `>=`-style end guards when an output cursor must never reach or pass a one-past-end pointer; and route failures through ownership cleanup whenever an acquired parser resource still needs release.

**Confidence:** Very high. Merged master wiretap memory-safety fix authored and merged by John Thacker and preserved in a stable-branch backport.

## Bound terminator searches to the enclosing protocol field

A protocol field that semantically contains a NUL-terminated string can still be malformed by omitting the terminator. When that string sits inside a length-delimited item, searching for the terminator must stop at the item's declared boundary rather than continuing into the next item.

Merged MR !26125 hardens SAP DIAG parsing by replacing an effectively unbounded string-size assumption with `tvb_strnlen()` bounded by `tvb_reported_length_remaining()` on the item's subset tvbuff. If no NUL appears before that boundary, the dissector reports expert information and treats only the bounded bytes as the field instead of consuming adjacent protocol data. Martin Mathieson's review also corrected an impossible negative check on the unsigned result of `tvb_reported_length_remaining()` and encouraged use of `proto_tree_add_item_ret_uint8()` when the value being displayed is also needed by the decoder.

**Implementation rule:** for terminator-delimited content nested inside an explicit-length structure, bound the terminator search by that structure. A missing terminator is malformed input, not permission to scan into the next field.

**Confidence:** Very high. Merged master parser-hardening change with detailed Martin Mathieson review and follow-up fuzzing by the contributor.

## Treat decompression as a bounded transformation and preserve the raw fallback

Compressed payloads combine attacker-controlled length metadata with potentially large allocations, reassembly state, and a secondary parser over generated bytes. Validate all framing and declared sizes before allocation or decompression, and make the generated output an optional enhancement rather than the only representation of the packet.

Merged MR !26126 enables SAP DIAG/RFC decompression using Wireshark's shared SAP LZC/LZH implementation. The accepted code validates compression headers, fragment/reassembly boundaries, and declared uncompressed sizes; applies configurable maxima (16 MiB by default for DIAG); decompresses RFC tables only when reassembly is complete; reports failures and length mismatches through expert information; exposes successful output as a separate data source; and preserves the available compressed/raw payload when decompression is disabled, fails, or is incomplete. The contributor also supplied a representative real SAP GUI capture before the MR was marked ready, and Anders Broman approved and merged it.

**Implementation rule:** validate compressed and uncompressed length metadata before allocating; impose a defensible configurable output ceiling; do not decompress incomplete logical units; and retain the original bytes when transformation is unavailable or unsuccessful. Successful decompression should be clearly represented as derived data rather than silently replacing the source bytes.

**Confidence:** Very high. Merged master feature with explicit resource bounds, malformed-input handling, a representative capture, and Anders Broman approval.