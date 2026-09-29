# Wireshark MR review findings — !4461–!4510

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 MRs were reviewed; the authoritative exact set is in `reviewed-mrs-automation-4461-4510.md`. Outcome: 48 merged and two closed/unmerged (!4499 and !4473). Closed work was down-weighted.

## Strongest findings

- **!4510 (merged master, John Thacker):** BT-DHT heuristic recognition was strengthened from a broad bencode prefix test to four exact five-byte protocol-structured prefixes derived from sorted legal keys and container types. That stronger recognizer justified changing the heuristic from default-disabled to default-enabled. The same MR corrected BEP 42's `ip` value to the six-byte IPv4+port form.
- **!4501 (merged master, Stig Bjørlykke):** Anders Broman requested `proto_tree_add_item_ret_uint()` instead of a separate fetch plus tree insertion. Use add-and-return APIs when parser logic needs the same field value shown in the tree.
- **!4500 (merged master, João Valverde):** `ws_assert_magic()` must follow the configured `WS_DISABLE_DEBUG` polarity. The series also routes diagnostics through wslog and replaces macro-generated accessors with explicit functions.
- **!4498 (merged master, João Valverde):** display-filter AST rewriting is simplified by a single ownership-aware `stnode_replace()` operation instead of allocate/free/re-parent sequences.
- **!4497 (merged master, Brian Sipos):** the BPv7/BPSec integration separates BPv6, BPv7, BPSec, and TCPCLv3 responsibilities, exposes intentional extension APIs, uses shared CBOR/wmem facilities, and adds committed capture-based tests.
- **!4482 (merged stable backport):** minizip compatibility is selected by probing the concrete `zip_fileinfo` member exposed by installed headers because distributions can package minizip-ng compatibility code under the old package identity.
- **!4476 (merged stable backport):** VSS Monitoring's weak port-stamping-only recognition remains default-disabled. Together with !4510 this gives a useful two-sided heuristic policy: default state should follow recognition strength.
- **!4474 (merged stable backport):** MPEG-TS continuity-counter loss belongs in `PI_SEQUENCE`, not `PI_MALFORMED`; sequence loss and malformed syntax are different diagnostic classes.
- **!4468 (merged master, Martin Mathieson):** `check_typed_item_calls.py --mask` found EtherCAT Boolean width/mask registration errors, independently reinforcing typed-item validation before submission.
- **!4467 / !4466 (merged stable backports):** a build-time Lemon generator must be built for the host and invoked by its CMake target path, not by the target compiler or ambient PATH.
- **!4465 (merged stable backport):** if capture CRC is already known bad, a later decrypt failure should not be diagnosed specifically as a MIC failure; report the causal upstream integrity problem.
- **!4462 (merged stable backport):** display-filter absolute-time serialization must use the same local-time semantics accepted by the parser so generated filters round-trip. Gerald Combs also required `g_assert_not_reached()` for impossible representation kinds.

## Lower-weight closed work

**!4499** was closed and later superseded by !4807. Its review is still useful negative evidence: Jaap Keuter caught a fixed-step decrement of an unsigned packet length that could wrap, requested fuzzing, required `value_string` terminators and handoff-time dissector lookup, while Alexis La Goutte requested scope splitting. **!4473** was a stable Diameter backport closed as not applicable because prerequisite support was absent.

There was **no substantive Guy Harris-authored change or review comment in this batch**, so no Guy-derived convention was inferred. Direct reviewer evidence that materially affected conclusions includes Anders Broman on !4501, Jaap Keuter and Alexis La Goutte on !4499, Gerald Combs on !4462, and Martin Mathieson's merged checker-driven !4468.

The remaining release/version bumps, registry updates, small field/offset fixes, stable branch backports, and UI/packaging corrections were examined for outcome, discussion, and change shape. They added corroboration but no stronger durable convention than the items above.
