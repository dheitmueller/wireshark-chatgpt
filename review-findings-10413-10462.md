
# Wireshark MR review findings — !10462 through !10413

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed merge requests, descending from !10462 through !10413. Selection was based on individual MR membership in the accumulated tracking, not on inferred numeric-range coverage. The previous !10463–!10512 ledger marks !10462 only as an unreviewed frontier probe, and searches of the review tracking found no prior completed review entries for !10461 through !10413. The historical !17571–!17620 batch remains preserved and counted.

Outcome weighting: 47 MRs were merged. The three closed/unmerged MRs were !10458, !10428, and draft !10420, and were given lower weight than merged work. No substantive Guy Harris review discussion appeared in this batch.

## Strong durable findings

### !10441 — SSH keylog cache lifetime and invalidation

Fabian Bäumer identified a lifetime mismatch between a persistent SSH key hash table/file handle and file-scoped key material that disappeared when the capture closed. John Thacker's review rejected the tempting workaround of rereading the keylog on every capture open because the cache is intentionally lazy and must also notice a keylog file that changes while Wireshark remains running. The accepted implementation moves hash entries to GLib-owned storage with table destructors, clears the cache when the keylog source is reopened or errors, and tears the process-lifetime table down at shutdown. Promoted to `architecture.md`.

### !10460 and !10423 — separate PDU semantics from carrier framing

John Thacker's !10460 separates a one-PDU BGP decoder from the TCP-oriented BGP wrapper so BMP, which already carries complete aligned BGP PDUs, can call the semantic decoder directly and continue parsing trailing BMP data using the returned byte count. !10423 independently adds an H.265 Annex-B bytestream entry point for MPEG-PES. Promoted to `transport-framing-conventions.md`.

### !10456 — historical state for random-access dissection

PostgreSQL interpretation depends on previously observed authentication/encryption state. John Thacker explicitly notes that a single "last state" value works only for sequential processing; the accepted code stores transitions in a tree so redissection can obtain the state valid at the packet being revisited. Promoted as historical corroboration to `dissector-state-conventions.md`.

### !10445 — reporting initialization must precede parsers that can report

A syntax error while reading recent/configuration files could call `report_warning()` before the reporting callbacks had been initialized, causing a null dereference. John Thacker moved `init_report_message()` before those files are read. Promoted to `initialization-lifecycle-conventions.md`.

### !10453 — named dissectors are a discoverability contract

David Perry's broad ASN.1 cleanup changes reusable dissectors from anonymous handles to `register_dissector()` identities so `find_dissector()`, Lua, rawshark, and fuzzshark can discover them, while intentionally leaving private directly-wired helpers anonymous. This strongly corroborates the existing registration-lifecycle rule in `initialization-lifecycle-conventions.md`.

### !10427 — standardized character sets belong in common decoding infrastructure

John Thacker implements EBCDIC CP500 centrally in charsets, tvbuff decoding, IANA mapping, introspection, and documentation before using it from DRDA. Promoted to new `text-encoding-conventions.md`.

## Strong corroboration retained without duplicate rules

- !10454 moved a shared unit string into common infrastructure but initially edited generated `packet-lpp.c`; Chuck Craft caught the duplicate-definition/build failure and pointed out that the ASN.1 template is authoritative. This reinforces the notebook's existing generated-source rule.
- !10438 received John Thacker review recommending `pinfo->pool` and wmem string helpers instead of deprecated ambient packet scope/manual free. This corroborates `allocator-scope-conventions.md`.
- !10439 contains Alexis La Goutte's explicit stable-branch guidance that fixes are candidates for backport while this new protocol support is an enhancement and should remain master-only. This corroborates `submission-backport-scope-conventions.md`.
- !10435 plus backports !10449/!10450 fix Community-ID tuple ordering by converting big-endian ports to host order before numeric comparison. Gerald Combs notes the cleanup of the local boolean type belongs on master while the correctness backport can proceed.
- !10425 fixes sFlow fields whose wire representation is a 32-bit big-endian quantity with only the low eight bits meaningful; treating the field as an 8-bit value at the first byte produced incorrect semantics. This reinforces exact wire-width/endian review.
- !10452 extends `check_val_to_str.py` to plugin dissectors as well as core dissectors, reinforcing that project static checks should cover supported plugin code too.
- !10418 upgraded Npcap to 1.74, and !10443 promptly reverted it after an upstream timestamp regression was reported. This is useful dependency-maintenance evidence: a nominally newer dependency does not outrank observed capture correctness.
- !10420 is an instructive but deliberately down-weighted negative result. João Valverde's recursive-display-filter proof of concept attracted useful use cases and extensive testing, but exposed correctness, multi-pass, memory, and severe performance concerns; João ultimately closed it saying it was not good enough to merge and wanted a stronger architecture. It is evidence for review discipline, not accepted architecture.
- !10428 is a closed iteration of the URL-checker caching work; the focused implementation subsequently appears as merged !10419, so the merged result carries the architectural weight.
- !10458 only added a trailing newline to `wka`; Martin Mathieson questioned the purpose and it closed unmerged. No durable convention was extracted.

## Next frontier

MR !10412 exists at the same corpus commit, is merged, and was inspected only to establish the next descending frontier. It was not reviewed or counted in this run.
