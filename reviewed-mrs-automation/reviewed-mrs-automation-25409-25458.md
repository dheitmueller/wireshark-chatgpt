# Automated Wireshark MR review: !25409–!25458

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to oldest. This run reviewed exactly 50 previously unreviewed merge requests, !25458 through !25409. Existing tracking in `reviewed-mrs-automation/` and `reviewed-mrs.md` was consulted before selecting the batch.

| MR | Status | Notes |
|---|---|---|
| !25458 | Deep | Zlib decompression cleanup removes redundant copy/zeroing and validates performance against pathological decompression inputs; merged. |
| !25457 | Scanned | GitLab fuzz artifact upload path fix; merged, no durable coding/review lesson beyond CI correctness. |
| !25456 | Scanned | O-RAN FH CUS missing-section handling; merged, reinforces bounded protocol-state handling. |
| !25455 | Scanned | SCSI cppcheck cleanup; merged, no additional durable lesson. |
| !25454 | Deep | HiPerConTracer heuristic strengthened using protocol-specific payload invariants and a supplied pcap; reinforces existing heuristic-validation guidance. |
| !25453 | Deep | HTTP malformed-header diagnostics. John Thacker explicitly distinguished `PI_PROTOCOL` for safely dissectable spec violations from `PI_MALFORMED` when parsing must give up. Promoted to protocol diagnostics conventions. |
| !25452 | Scanned | Additional dissector cppcheck fixes; merged, no new lesson beyond existing static-analysis guidance. |
| !25451 | Deep | TLS hash removed undefined unaligned wider loads exposed by UBSan. Promoted alignment-safe byte-access convention. |
| !25450 | Deep | UDP/DCCP Follow Stream captures addresses, ports, and stream ID at tap time because final `packet_info` values can reflect tunneling. Promoted with !25426. |
| !25449 | Scanned | sharkd abstract Unix sockets restrict peers to same eUID using `SO_PEERCRED`; merged security hardening. |
| !25448 | Scanned | sharkd uses AF_UNIX on supported Windows versions; merged portability work. |
| !25447 | Scanned | sharkd TCP listener restricted to loopback because transport lacks authentication; merged security boundary. |
| !25446 | Scanned | release-4.4 Qt 6.11 deprecation backport; no additional lesson. |
| !25445 | Scanned | release-4.4 DBS Etherwatch buffer-slack backport; same lesson as !25436. |
| !25444 | Scanned | HTTP QUERY method / Accept-Query RFC update; merged, protocol table update only. |
| !25443 | Scanned | SECURITY.md markup correction; merged, no engineering lesson. |
| !25442 | Discussion-focused | Broad cppcheck cleanup. Martin Mathieson notes cppcheck is invoked by `check_dissector.py` and caught confusing unsigned comparisons; reinforces existing static-analysis/pre-submit guidance. |
| !25441 | Partial (open draft) | TLS-over-COTP experiment documents conversation scoping and several fragmentation layers. Useful provisional context only; not promoted as accepted architecture. |
| !25440 | Scanned | Unsigned offset cleanup in remaining dissectors; merged, part of broad type-domain migration. |
| !25439 | Scanned | release-4.6 DBS Etherwatch buffer-slack backport; no additional lesson. |
| !25438 | Scanned | release-4.6 dead-store backport found by Clang Analyzer; reinforces static-analysis practice. |
| !25437 | Scanned | SECURITY.md asks reporters to prefer confidential issues to email; merged project-process update. |
| !25436 | Deep | DBS Etherwatch reserves enough buffer slack for one line written before the parser's post-write length check; strong example of sizing for actual write order, consistent with existing buffer-capacity guidance. |
| !25435 | Scanned | Unsigned offset cleanup in ASN.1 dissectors; merged. |
| !25434 | Scanned | Unsigned offset cleanup in leftover dissectors; merged. |
| !25433 | Scanned | Unsigned offset cleanup in P–Z dissectors; merged. |
| !25432 | Scanned | Unsigned offset cleanup in K–O dissectors; merged. |
| !25431 | Scanned | Unsigned offset cleanup in F–J dissectors; merged. |
| !25430 | Discussion-focused | Unsigned offset cleanup in C–E dissectors with follow-up discussion; reinforces keeping comparisons consistent with the variable's semantic/type domain. |
| !25429 | Scanned | Generated `tpncp.dat` update; merged, no durable lesson. |
| !25428 | Deep | COTP stores source/destination references learned during connection setup because later TPDUs omit them; useful stateful-conversation exemplar. |
| !25427 | Scanned | Decode As MSVC external-symbol/link fix; merged portability correction. |
| !25426 | Deep | TCP Follow Stream uses TCP-layer addresses/ports captured at tap time instead of final mutable `packet_info`; promoted with !25450 to conversation identity conventions. |
| !25425 | Scanned | Removed stale, untestable Travis CI configuration; merged project-maintenance cleanup. |
| !25424 | Scanned | Unsigned offset cleanup in 0–9/A/B dissectors; merged. |
| !25423 | Scanned | release-4.4 COTP checksum-length fix; backport of !25421. |
| !25422 | Scanned | release-4.6 COTP checksum-length fix; backport of !25421. |
| !25421 | Deep | COTP checksum must cover entire TPDU rather than checksum TLV length; merged correctness fix and backported. |
| !25420 | Discussion-focused | New Qt statistics Distribution dialog; merged after iterative review. Mostly GUI/statistics-specific, no notebook-wide rule extracted. |
| !25419 | Deep | COTP reassembly keeps per-flow, per-direction fragment IDs and enough persistent state for arbitrary redissection order. Promoted to stateful reassembly conventions. |
| !25418 | Scanned | tshark `-E split=<proto>` per-message-instance output; merged feature, no broadly reusable lesson extracted. |
| !25417 | Discussion-focused (closed) | Proposed 22-dissector sentinel-scan bounds cleanup; closed unmerged. The captured-length idiom is useful but lower-weight and already represented by accepted bounds guidance. |
| !25416 | Scanned | Capture progress status formats sizes with existing formatter; merged UI cleanup. |
| !25415 | Discussion-focused | Removed Qt Core5Compat dependency using GLib conversion; Gerald Combs supplied implementation-cleanup review. Merged, mostly Qt/dependency-specific. |
| !25414 | Deep | Guy Harris-authored BSD setup modernization probes platform package names/capabilities rather than assuming one naming scheme; merged, reinforces portability practice. |
| !25413 | Scanned | O-RAN FH CUS improves U-plane/C-plane matching; merged protocol-specific state matching. |
| !25412 | Scanned | Restores Qt progress status strings emitted by C routines; merged UI behavior fix. |
| !25411 | Scanned | Debian changelog timestamp correction; merged packaging maintenance. |
| !25410 | Deep | Hot-path optimizations were accompanied by measured before/after timings and perf attribution; merged, reinforces evidence-driven optimization practice. |
| !25409 | Scanned | README.tapping function-signature documentation update; merged, no additional durable lesson. |

## Durable notebook updates from this run

- `conversation-identity-conventions.md`: capture protocol-layer stream identity at tap time rather than reconstructing it from mutable final `packet_info` state (!25426, !25450).
- `stateful-reassembly-conventions.md`: maintain direction-specific reassembly identifiers and enough persistent state to reproduce first-pass decisions under arbitrary redissection order (!25419).
- `protocol-input-diagnostics-conventions.md`: use `PI_PROTOCOL` for specification violations that remain safely dissectable and reserve `PI_MALFORMED` for malformed structure that forces dissection to stop (!25453, direct John Thacker review guidance).
- `c-wire-layout-portability-conventions.md`: do not dereference wider integer casts from byte buffers unless alignment is guaranteed; use alignment-safe operations instead (!25451, UBSan-backed John Thacker fix).

No attempt was made to treat open !25441 or closed !25417 as accepted architecture; both were weighted below merged evidence.