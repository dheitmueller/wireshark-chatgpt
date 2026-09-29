# Review findings — MRs 4611–4660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Outcome: 48 merged, 2 closed/unmerged (!4658 and !4657). Merged master changes carry the strongest weight; stable backports mainly corroborate master changes; closed or superseded submissions are lower-weight history.

## Strong findings

- **!4645 — global `wireshark.h` architecture.** João Valverde's merged master change documents that the umbrella header must stay minimal, configuration-independent, limited to installed Wireshark headers, and free of upward component dependencies. Graham Bloice raised the “one big header” risk. Guy Harris then asked whether C files had any reason to include assert/log/wmem headers directly once those facilities are intentionally supplied by `wireshark.h`; João answered no. Treat this as a narrow global-baseline rule, not permission for broad transitive includes.
- **!4616 — centralize packet accounting at the successful-write boundary.** Guy Harris moves captured/written/sync-pipe increments into `capture_loop_wrote_one_packet()` instead of duplicating them in read/dequeue paths. Stable !4618, !4623 and !4631 corroborate it. This is strong guidance for threaded accounting: count the semantic event where it is committed.
- **!4612 → !4619/!4620 → !4625/!4626 — build-graph validation.** Gerald Combs's documentation-build speedup exposed macOS and Windows failures. Guy Harris repaired the macOS bundle-resource handling and reported the Windows failure; the target refactor was then partially reverted. Later reviewed MRs provide the stronger final build design, so this sequence is mainly negative evidence that generated-artifact target changes must be tested on every packaging/build generator that consumes them.
- **!4647 + !4632 + !4649 — eliminate ambiguous display-filter text.** `matches` now requires a quoted string; invalid unquoted protocol byte text fails instead of silently becoming a string; the one-byte `0xNN` convenience moves into byte conversion instead of a relation-specific semantic special case. User-language changes were paired with tests and release-note updates.
- **!4638 — reuse canonical BitTorrent PDU framing over uTP.** John Thacker wraps the existing BitTorrent PDU length/dissection callbacks so multiple application PDUs can be handled over uTP without duplicating application parsing.
- **!4648 — precise commit subjects.** João Valverde requested the final headline `wsutil: install missing public header wsgcrypt.h`, insisting that the subject name the correct file/component and explain the actual operation.
- **!4629 / !4630 — original SocketCAN FDF support.** Guy Harris's master implementation and stable backport are real accepted history, but later already-reviewed !4715 hardens the logic for old captures whose formerly reserved bytes may contain garbage. !4715 is the stronger compatibility precedent.
- **!4633 — disabled debug output does not imply compiled-out arguments.** Declarations referenced in `ws_debug()` calls still have to exist under `WS_DISABLE_DEBUG`. This corroborates the later, stronger !4701 rule.
- **!4652 — RPC defragmentation guard.** Stable backport evidence for only invoking deeper defragmentation when the transport context can actually supply the full record fragment; master origin !4283 should carry primary weight when reviewed.
- **!4655 — stronger BT-uTP heuristic.** Stable backport adds extension/window plausibility checks and optional legacy-v0 recognition. This corroborates conservative heuristic ownership; master origin !4533 should carry primary weight when reviewed.
- **!4644 — Follow Stream overlapping-read guard.** Roland Knall questioned unnecessary work before the guard and putting one-use state in the class header; the author later submitted !4767 to keep the helper local. The later cleanup is the stronger code-organization outcome.

## Per-MR index

| MR | Outcome | Depth | Note |
|---|---|---|---|
| !4660 | merged | Scanned | BPv7/BPSec release-3.6 backport. |
| !4659 | merged | Scanned | RDPUDP AckVec spec update. |
| !4658 | closed | Scanned | duplicate TECMP backport; changes already present. |
| !4657 | closed | Scanned | duplicate ISO15765 backport; changes already present. |
| !4656 | merged | Scanned | Qt Q_OBJECT cleanup backport. |
| !4655 | merged | Deep | BT-uTP heuristic strengthening; stable backport. |
| !4654 | merged | Scanned | OptoMMP memory ranges. |
| !4653 | merged | Scanned | NEWS/link maintenance. |
| !4652 | merged | Deep | RPC defragmentation precondition; stable backport. |
| !4651 | merged | Scanned | LISP strings moved to packet pool. |
| !4650 | merged | Scanned | BT-DHT BEP 42 support. |
| !4649 | merged | Deep | one-byte hex handling moved to byte parser. |
| !4648 | merged | Discussion | commit-message wording review. |
| !4647 | merged | Deep | display-filter regex grammar tightening. |
| !4646 | merged | Scanned | release-3.6 bug-fix bundle. |
| !4645 | merged | Deep/high-authority | umbrella-header architecture; Guy review. |
| !4644 | merged | Discussion | Follow Stream reentrancy; later cleanup !4767. |
| !4643 | merged | Scanned | automatic update. |
| !4642 | merged | Scanned | automatic update. |
| !4641 | merged | Scanned | automatic update. |
| !4640 | merged | Scanned | automatic update. |
| !4639 | merged | Scanned | removes pointless bencode recursion. |
| !4638 | merged | Deep | BitTorrent PDU reuse over uTP. |
| !4637 | merged | Scanned | dftest docs. |
| !4636 | merged | Scanned | semcheck comment update. |
| !4635 | merged | Scanned | reserved filter names / leading dash validation. |
| !4634 | merged | Scanned | dfilter tests. |
| !4633 | merged | Deep | WS_DISABLE_DEBUG build fix. |
| !4632 | merged | Deep | unparsed dfilter semantics made explicit. |
| !4631 | merged | Scanned | packet-count centralization backport. |
| !4630 | merged | Scanned | SocketCAN FDF backport. |
| !4629 | merged | Deep/high-authority | Guy Harris SocketCAN FDF master change. |
| !4628 | merged | Scanned | stats-tree CLI syntax. |
| !4627 | merged | Scanned | duplicated syntax-node crash fix. |
| !4626 | merged | Scanned | docs target partial revert backport. |
| !4625 | merged | Deep | docs target partial revert. |
| !4624 | merged | Scanned | resolve field names in parser. |
| !4623 | merged | Scanned | packet-count backport. |
| !4622 | merged | Scanned | threaded receive double-count fix. |
| !4621 | merged | Scanned | threaded receive double-count fix. |
| !4620 | merged | Scanned | macOS doc-build repair backport. |
| !4619 | merged | Deep/high-authority | Guy Harris macOS doc-build repair. |
| !4618 | merged | Scanned | packet-count backport. |
| !4617 | merged | Scanned | threaded receive double-count fix. |
| !4616 | merged | Deep/high-authority | Guy Harris packet-accounting centralization. |
| !4615 | merged | Scanned | docs build speedup backport. |
| !4614 | merged | Scanned | captype docs. |
| !4613 | merged | Scanned | version bump. |
| !4612 | merged | Deep | docs build speedup; Guy reports Windows failure. |
| !4611 | merged | Scanned | captype docs. |
