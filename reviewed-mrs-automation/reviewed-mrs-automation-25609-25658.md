# Automated Wireshark MR review: !25609 through !25658

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to older.

Exactly 50 previously unreviewed MRs were examined in this run: !25658 through !25609 inclusive. Before selection, the existing per-run ledgers in `reviewed-mrs-automation/`, the supplemental automation ledger, and `reviewed-mrs.md` were consulted/search-checked. No prior review entry was found for any MR in this 50-MR block. The previously reviewed !17571-!17620 batch remains preserved and counted as reviewed.

Review-depth meanings follow the notebook convention: **Deep** means the implementation/diff and useful discussion were examined closely enough to derive or corroborate durable engineering guidance; **Discussion-focused** means reviewer discussion was the main useful evidence; **Scanned** means metadata, outcome, diff purpose, and discussion were checked but no distinct durable lesson justified promotion.

| MR | Outcome | Depth | Review note |
|---|---|---|---|
| !25658 | Merged | Scanned | release-4.4 backport removing an unnecessary sdjournal variable; no new durable lesson. |
| !25657 | Merged | Scanned | release-4.6 backport of the sdjournal dead-variable cleanup; no additional lesson. |
| !25656 | Merged | Scanned | master sdjournal dead-variable cleanup by Guy Harris; straightforward cleanup. |
| !25655 | Merged | Deep | Guy Harris makes wiretap bytes-dumped state unsigned 64-bit; strong evidence that inherently non-negative byte counts should use a non-negative type with sufficient range. Promoted to `api-domain-conventions.md`. |
| !25654 | Merged | Deep | HTTP/3 avoids recreating file-scoped upgrade information during redissection and fixes direction state. Promoted as redissection/lifetime guidance in `dissector-context-flow-conventions.md`. |
| !25653 | Merged | Deep | John Thacker changes proto APIs returning lengths to unsigned types because results are intrinsically non-negative. Promoted to `api-domain-conventions.md`. |
| !25652 | Merged | Scanned | O-RAN list traversal checks lookup result before walking; useful defensive fix but already covered by existing nullability/invariant guidance. |
| !25651 | Merged | Deep | Guy Harris moves wiretap byte-count arithmetic to unsigned semantics; corroborates the API-domain rule promoted from !25655/!25653. |
| !25650 | Merged | Discussion-focused | Base64url decoding accepts optional padding; Jaap Keuter review simplified length arithmetic before merge. Useful local implementation review, no separate notebook rule. |
| !25649 | Merged | Deep | ADSP dissector added with a real Mac OS 9/printer capture and implementation validation; strong corroboration of the existing sample-capture expectation for new protocol work. |
| !25648 | Merged | Scanned | release-4.4 backport correcting gcrypt terminology/comment; no new engineering convention. |
| !25647 | Merged | Scanned | release-4.6 backport of the same gcrypt comment correction. |
| !25646 | Merged | Scanned | master gcrypt terminology/comment correction by Guy Harris; no distinct implementation lesson. |
| !25645 | Merged | Scanned | ITS Automated Vehicle Marshalling/Parking ASN.1 update; largely generated/spec-driven and no substantive reusable review guidance. |
| !25644 | Merged | Scanned | O-RAN C-plane/U-plane linkage update; merged without a distinct new notebook convention. |
| !25643 | Merged | Deep | DoH GET support saves/restores dissector dispatch context while invoking DNS; corroborates existing context-preservation guidance. |
| !25642 | Merged | Deep | QUIC retransmission analysis is gated to the first PDU in a frame because the state belongs to the enclosing frame, not every child PDU. Promoted to `dissector-context-flow-conventions.md`. |
| !25641 | Merged | Deep | QUIC explicitly passes FIN state to stream subdissectors via `quic_stream_info`. Promoted as parent-to-child context propagation guidance. |
| !25640 | Merged | Scanned | Qt logging-domain cleanup; no durable cross-cutting lesson. |
| !25639 | Merged | Scanned | O-RAN uplink C/U-plane linkage work; no distinct review convention extracted. |
| !25638 | Merged | Scanned | dissector warning cleanup; no new lesson beyond existing warning/static-analysis guidance. |
| !25637 | Merged | Discussion-focused | John Thacker explicitly prefers `tvb_*_length_remaining(tvb, offset + n)` over manual subtraction because the helper safely handles underflow. Strong corroboration of an already-recorded tvbuff remaining-length rule. |
| !25636 | Merged | Scanned | release-4.6 backport of ringbuffer close-result correction; same close-status semantics already recorded from nearby Guy Harris work. |
| !25635 | Merged | Deep | Guy Harris fixes inverted close-success testing in ringbuffer output; corroborates the notebook's existing successful-finalization/error-close API-status guidance. |
| !25634 | Merged | Scanned | Follow HTTP/3 Stream implementation reuses/exposes QUIC follow helpers; useful feature work but no distinct convention beyond helper reuse. |
| !25633 | Merged | Scanned | release-4.6 file-wrapper error-info backport; diagnostic improvement without a new general rule. |
| !25632 | Merged | Scanned | wsgcrypt cleanup/backport; formatting/comment correctness only. |
| !25631 | Merged | Scanned | AFP dead-store cleanup found by Clang Analyzer; corroborates existing static-analysis practice. |
| !25630 | Merged | Deep | JSON protocol-scoped wildcard matching moves lookup/state to matching protocol scope and resets active state per packet. Promoted to `dissector-context-flow-conventions.md`. |
| !25629 | Merged | Discussion-focused | Qt find-in-packet follow-up repairs issues after earlier work merged before review was complete; useful cautionary history but not promoted as a universal rule. |
| !25628 | Merged | Scanned | release-4.4 backport widening RELOAD offsets; corroborates the master fix. |
| !25627 | Merged | Scanned | release-4.6 backport widening RELOAD offsets; corroborates the master fix. |
| !25626 | Merged | Scanned | whitespace cleanup by Guy Harris; no durable technical lesson. |
| !25625 | Merged | Deep | RELOAD parser offsets widened after 32-bit packet quantities were stored in 16-bit variables. Promoted as semantic-domain/range evidence in `api-domain-conventions.md`. |
| !25624 | Merged | Deep | Follow Stream dialog reworked to event-driven behavior rather than assuming packet updates complete synchronously; strong UI architecture example, but kept in ledger rather than creating a new UI-specific notebook rule from one MR. |
| !25623 | Merged | Scanned | GitLab CI manuf header/update-frequency text; no durable development convention. |
| !25622 | Merged | Deep | Internal proto helpers become unsigned while the outer API layer converts the supported `-1` sentinel to a real remaining length. Strong boundary-normalization exemplar promoted to `api-domain-conventions.md`. |
| !25621 | Merged | Scanned | release-4.4 backport of geometry ownership/double-free fix; corroborates !25616. |
| !25620 | Merged | Scanned | release-4.6 backport of geometry ownership/double-free fix; corroborates !25616. |
| !25619 | Merged | Scanned | O-RAN v21 section-extension dissection; no new cross-cutting review lesson. |
| !25618 | Merged | Scanned | release-4.4 backport of MEGACO doubled-comma infinite-loop fix; corroborates master. |
| !25617 | Merged | Scanned | release-4.6 backport of MEGACO doubled-comma infinite-loop fix; corroborates master. |
| !25616 | Merged | Deep | Window-geometry hash entries now own an independent string copy and the Qt temporary backing buffer is kept alive until copying completes. Promoted to `ownership-copy-conventions.md`. |
| !25615 | Merged | Deep | MEGACO doubled-comma infinite-loop fix ties termination to the cursor that actually advances. Strong corroboration of the notebook's existing monotonic parser-loop rule. |
| !25614 | Merged | Scanned | MEGACO/TPKT fixes inverted tests for encapsulation and fragmentation length; correctness fix without a new general convention. |
| !25613 | Merged | Scanned | release-4.4 backport of H.265 returned-bit-offset fix; corroborates master. |
| !25612 | Merged | Scanned | release-4.6 backport of H.265 returned-bit-offset fix; corroborates master. |
| !25611 | Merged | Deep | UET CRC unit-test MR adds generated coverage across IPv4/IPv6/UDP combinations; later John Thacker identified consistent big-endian failure, which led to the separate endian CRC correction already represented in notebook guidance. Useful corroboration of cross-architecture testing. |
| !25610 | Merged | Deep | H.265 caller now consumes the bit offset returned by a parsing helper instead of discarding parser-position state. Useful parser-cursor exemplar; existing parser-state/progress guidance was sufficient. |
| !25609 | Merged | Deep | New CoAP-EAP RFC 9820 dissector with delegated EAP dissection, shared wscbor use, parent blockwise handling, two captures, 20 tests, explicit limitations, AI-assistance disclosure, and maintainer acceptance. Strong new-dissector/testing exemplar; existing notebook conventions already cover the durable pieces. |

## Durable notebook updates from this run

- `api-domain-conventions.md`: semantic unsigned domains for lengths/counts/offsets and normalization of compatibility sentinels at API boundaries, based primarily on !25655, !25653, !25651, !25625, and !25622.
- `dissector-context-flow-conventions.md`: explicit parent-to-child framing metadata, once-per-enclosing-frame analysis, and redissection/matcher state scoping, based on !25641, !25642, !25654, and !25630.
- `ownership-copy-conventions.md`: container-owned pointer members must receive independent copies at the ownership-transfer boundary, based on !25616 and its stable-branch backports !25620/!25621.

Several other strong observations were deliberately not duplicated because the notebook already records them: use tvbuff remaining-length helpers instead of subtraction (!25637), require monotonic parser-loop progress (!25615), and test/check endian-sensitive checksum behavior across architectures (!25611 corroborating the later !25985 fix).