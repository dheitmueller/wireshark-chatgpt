# Durable conventions from Wireshark MRs 4761–4810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Multi-valued display-filter inequality

Merged !4810 makes `a != b` the logical negation of `a == b`. For a multi-valued field that means every value must differ. The older existential behavior remains separately spelled `~=` / `any_ne`.

**Rule:** if one language operator is defined as the negation of another, preserve that algebraic relationship over collections. Give a different spelling to a legacy operation with different quantifier semantics.

## Framework callback sentinels are part of the API contract

Merged !4778 documents that a zero PDU length from a `tcp_dissect_pdus()` length callback means “length cannot yet be determined; request more data.” DCERPC therefore cannot return an invalid on-wire zero fragment length unchanged; it substitutes another invalid value below the fixed-header minimum so the framework diagnoses malformed framing.

**Rule:** before returning a packet-derived integer through a framework callback, check whether that numeric value is also a control sentinel.

## Missing reassembly is a runtime bounds condition

Merged !4787 replaces an assertion with `FragmentBoundsError` when more TCP data is requested but reassembly is unavailable. Merged !4765 likewise gives fragment bounds classification priority when the tvbuff is known to represent an unreassembled fragment.

**Rule:** reserve assertions for programming invariants. States reachable through preferences, checksum policy, truncation, or unavailable reassembly should use normal runtime/packet exception semantics.

## Packet-controlled loops must prove progress

Merged !4782 breaks ORAN extension parsing when reserved `extlen == 0` would otherwise repeat at the same offset. In !4807, Jaap Keuter identifies related malformed option-length rollover/no-progress risk.

**Rule:** before repeating a packet-length-driven loop, prove the chosen stride is valid and advances the cursor; otherwise diagnose and terminate.

## Heuristics should validate framing length

Merged !4778 strengthens DCERPC recognition by requiring the complete fixed header and a declared fragment length at least that large.

**Rule:** heuristic ownership tests should include cheap self-consistency constraints such as minimum legal PDU length, not only version or type bytes.

## TLV width should come from the encoded TLV

Merged !4763 initially widened the TZSP channel field to two bytes, breaking old one-byte captures. Alexis La Goutte redirected the implementation to decode using the TLV's actual length, and both encodings were tested.

**Rule:** when old and new TLV widths are both legal, read the width from the TLV rather than hard-coding only the newest producer format.

## Project checkers are part of dissector review

The !4807 review uses project pre-commit/checkhf/API checks, compiler warnings, and Clang analyzer findings to catch field-registration, naming, encoding, prototype, and parser-loop problems.

**Rule:** a clean build on one toolchain is not enough; run the project-supplied checker/static-analysis suite before submission.

## Couple displayed scalar decoding with parser use

Jaap Keuter's !4807 review asks for the `proto_tree_add_item_ret_uint*` family where the same field is both displayed and needed by parser logic.

**Rule:** prefer add-and-return field APIs over a separate packet read plus display operation when the APIs support the field type.

## Child fallback needs the child's actual consumption result

Merged !4781 and !4780 correct COSE/BPv7 fallback paths so a disabled or rejecting child can return zero and generic CBOR/heuristic fallback can run.

**Rule:** when fallback depends on whether a child accepted data, use a dispatch API that preserves rejection/consumption semantics.

## Return the extent actually dissected

Merged !4786 makes CBOR return its final parser offset and sets the root item to the same extent instead of claiming the whole captured tvbuff.

**Rule:** where callers use a dissector return value for chaining or fallback, return the semantic consumed extent unless the API explicitly defines otherwise.

## Capture-size limits follow record framing

Merged !4802 raises USB capture limits because capture records model OS-level USB requests, not individual bus transactions. Guy Harris explicitly challenges the platform evidence and points out the related libpcap boundary.

**Rule:** choose wiretap/capture limits from the producer's record semantics and platform capabilities, and audit parallel producer/consumer limits together.
