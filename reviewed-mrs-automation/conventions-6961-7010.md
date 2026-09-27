# Durable conventions extracted from !6961–!7010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !7010 through !6961 after reconciling all available review tracking. Current upstream source remains authoritative.

## Assertions and malformed input

**Do not use `DISSECTOR_ASSERT` to report a protocol error.** In merged !6981, Jaap Keuter states this directly during BPv7/CBOR review. Packet-controlled invalid structure is handled by parser validity returns, normal bounds behavior, or expert/protocol diagnostics. Assertions remain for programmer invariants and impossible internal states.

## Conversation identity

Merged !6979, authored by Gerald Combs, adds `conversation_new_full()` and `find_conversation_full()` for arbitrary typed identity-element lists. Elements can include addresses, strings, unsigned integers, and unsigned 64-bit integers, terminated by an endpoint-type element. This lets protocols define conversation identity from their actual semantic identifiers instead of forcing every identity into an address/port tuple.

When such an identity is retained, **deep-copy key data into storage with the conversation's lifetime**. !6979 copies nested address/string storage into file scope before inserting the key into retained maps. Merged !7001 then moves by-ID helpers onto the generalized element model and expands IDs to 64 bits.

## Logical extent versus backing storage

Merged !6993 fixes FT_PROTOCOL negative slicing by carrying the protocol item's logical length in its fvalue. The old implementation inferred the end from the captured length of a backing TVB that could extend through later protocol layers. The accepted change also propagates later `proto_item_set_len()` updates into the fvalue.

**Rule:** an object representing a logical subrange must carry that subrange's semantic extent when its backing object can be larger.

## Compiler false positives

Merged !6984 is strong review evidence from John Thacker, João Valverde, and Guy Harris. After the GCC 12.1/Qt warning was analyzed as a compiler false positive, the accepted code uses Wireshark's `DIAG_OFF` / `DIAG_ON` helpers only around affected expressions, with a compiler-version condition and explanatory comments.

**Rule:** when a warning is understood to be false, prefer a narrow documented project-standard suppression over globally weakening the warning or changing correct code into a less idiomatic workaround.

## External tool and dependency capabilities

In merged !6994, John Thacker caught that a Qt resource-compiler switch was introduced only in Qt 5.13. The accepted CMake passes it only when the discovered Qt version supports it.

**Rule:** gate external command-line switches and APIs by the version/capability that actually introduced them.

## Integer domain and size APIs

Merged !7003 changes counters, widths, lengths, row/column counts, and related values from signed integers to unsigned types. Guy Harris explicitly questioned signed types for quantities guaranteed nonnegative; John Thacker noted that a negative signed value converted to an allocation-size domain can become unexpectedly huge.

**Rule:** choose integer types from the semantic domain and downstream API. Avoid accidental signed-to-size conversion for intrinsically nonnegative counts and sizes. This is not a mandate to convert every positive-valued integer mechanically.

## Protocol-tree API choice

During merged !6969, Alexis La Goutte requested `proto_tree_add_item()` for values directly represented by packet bytes and explained that explicit value-setting helpers are for values calculated separately.

**Rule:** let registered fields decode their wire-backed bytes through the standard item API whenever possible. Add computed/reconstructed values separately and mark them generated when appropriate.

## wmem scopes

Merged !6968, authored by Gerald Combs, documents the core allocator lifetimes:

- packet scope: normally released at the end of packet dissection;
- file scope: normally released when the capture file closes;
- epan scope: normally released when epan shuts down, usually at program exit.

**Rule:** choose the narrowest scope that matches the required lifetime, and never retain shorter-lived pointers in longer-lived state.

## Vendored third-party code

Merged !6995 brought lrexlib/PCRE2 code into the tree. Roland Knall still applied Wireshark's prohibited-API review to the vendored source. João Valverde correctly pointed out that mechanically replacing a matching deallocator would be wrong when it must pair with the allocator used by the upstream/library implementation.

**Rule:** vendored code is not exempt from project security/build review, but compliance changes must preserve upstream ABI and allocator-pairing contracts.

## Static checker confidence

Merged !6986 changes `check_typed_item_calls.py` so mask checks run only when the checker successfully parsed the mask expression.

**Rule:** distinguish “could not parse this expression” from “parsed it and proved it invalid.” Avoid hard correctness errors from an analysis result the checker did not actually derive.

## Review and submission corroboration

Merged !7000 supplied a sample capture for a new dissector. Merged !6980 was asked for both a motivating issue and sample capture for a new statistics UI. In !6985 Guy Harris requested a discoverable protocol specification reference.

**Rule:** new protocol behavior and protocol-specific UI/statistics work should come with reproducible sample traffic where practical, and protocol revisions should identify an authoritative specification/source when available.
