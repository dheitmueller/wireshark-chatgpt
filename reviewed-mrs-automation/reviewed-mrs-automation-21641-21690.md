# Wireshark MR automation review: !21641-!21690

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to oldest
MRs reviewed in this run: 50

Selection was built from the union of the existing `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, not from an assumed numeric boundary. The historical !17571-!17620 batch remains part of the already-reviewed set. No previously reviewed sparse exception was found inside this candidate block, so the fifty highest-numbered previously unreviewed corpus entries were exactly !21690 through !21641.

## Exact reviewed MR set

- !21690
- !21689
- !21688
- !21687
- !21686
- !21685
- !21684
- !21683
- !21682
- !21681
- !21680
- !21679
- !21678
- !21677
- !21676
- !21675
- !21674
- !21673
- !21672
- !21671
- !21670
- !21669
- !21668
- !21667
- !21666
- !21665
- !21664
- !21663
- !21662
- !21661
- !21660
- !21659
- !21658
- !21657
- !21656
- !21655
- !21654
- !21653
- !21652
- !21651
- !21650
- !21649
- !21648
- !21647
- !21646
- !21645
- !21644
- !21643
- !21642
- !21641

## Durable findings promoted

- **!21653 — Deep / merged stable backport of John Thacker change / high weight.** LLMNR multicast requests receive unicast responses, so an ordinary full endpoint conversation cannot join the two directions. The accepted code keys state by the stable requester address/port and transport type. Promoted to `conversation-identity-conventions.md`: choose conversation identity from the protocol relationship that actually shares state, not mechanically from the full packet tuple.
- **!21641 — Deep / merged master / very high weight.** John Thacker fixes SMB2 parsing so the GUID is always dissected and the parser offset always advances even when optional saved filename/state is absent. Promoted to `parser-state-boundary-conventions.md`: optional bookkeeping must not guard mandatory wire consumption.
- **!21651 (with !21648/!21664 cherry-picks) — Deep / merged safety fix / high weight.** DOF string construction now terminates after the bytes actually written when malformed length semantics disagree. Promoted to `parser-state-boundary-conventions.md`; repeated backports were treated as corroboration rather than independent evidence.
- **!21663 — Deep / merged master / very high weight.** John Thacker fixes Lua table serialization under non-C numeric locales by selecting the canonical C numeric locale during serialization and adds an explicit German-locale test. Promoted to `locale-serialization-conventions.md`.

## Strong corroborating review evidence

- **!21667 — Deep / merged master.** AMS request/response tracking adds a representative pcap. Anders Broman asks the contributor to use `proto_tree_add_item_ret_uint` rather than separately fetching the same InvokeId and later asks for a single squashed commit. In follow-up, Anders explicitly says it is better to amend the MR commit while addressing feedback rather than accumulating review-fix commits. This corroborates the notebook's existing fetch-once and submission-history guidance.
- **!21669 — Discussion-focused / merged master.** Alexis La Goutte initially flags an ASTERIX edit on the assumption that the file is generated; Gerald Combs clarifies that after the generator restructuring only `packet-asterix-generated.h` is generated, so the direct edit is valid. Durable lesson is to verify the *current* generator/source-of-truth boundary before demanding generator changes; do not apply historical generated-file assumptions mechanically.
- **!21681 — Deep / merged master.** Capture start/end timestamps can be represented both by absence and by the `nstime_t` unset sentinel. The fix normalizes both before presentation and initializes `file_end_ts` to the sentinel. Corroborates explicit sentinel initialization and checking the semantic API state rather than pointer presence alone.
- **!21687 — Deep / merged master.** O-RAN FH CUS removes duplicated preferences that represented array/cardinality dimensions already supplied by other configuration, reducing independent values that could disagree. Corroborates keeping one source of truth for state that controls bounds/array interpretation.
- **!21688 — Scanned / merged master.** `guid-utils` replaces aliasing macros with native calls while retaining one GUID-oriented vocabulary so usages stay searchable. Useful naming/API cleanup but not promoted as a new broad rule.
- **!21685 — Scanned / open draft.** SGP.22 v3.1 work remained draft/open in the corpus snapshot and was down-weighted as implementation evidence.

## Per-MR audit notes

- !21690 — merged release-4.4 macOS Liquid Glass compatibility backport; platform/release maintenance.
- !21689 — merged release-4.6 counterpart of !21690; no independent lesson.
- !21688 — merged GUID utility macro-to-native-call cleanup; naming/searchability rationale, no new rule.
- !21687 — merged O-RAN preference/cardinality consolidation; single-source-of-truth corroboration.
- !21686 — merged SGP32 column update simplification from formatted-string path to direct string path; local API/style cleanup.
- !21685 — open draft SGP.22 v3.1 upgrade; explicitly down-weighted.
- !21684 — merged stable backport of SGP.22 v2.6 update; generated/protocol maintenance.
- !21683 — merged GitLab CI removal of obsolete workaround; CI maintenance.
- !21682 — merged stable Qt 6.9.3 CI dependency update; build maintenance.
- !21681 — merged John Thacker capture-time unset/sentinel correctness fix; deep review.
- !21680 — merged master SGP.22 v2.6 update; protocol/generated-data maintenance.
- !21679 — merged CQLv5 non-compressed-frame support; protocol feature in a staged implementation series.
- !21678 — merged master Qt 6.9.3 CI update; build maintenance.
- !21677 — merged stable media-type registration for SGP32; dispatch/backport maintenance.
- !21676 — merged XnAP private-IE dissection support; protocol extension, no separate durable review rule extracted.
- !21675 — merged first CQLv5 support stage; staged protocol implementation.
- !21674 — merged NR RRC v18.7.0 generated-dissector upgrade; generated protocol maintenance.
- !21673 — merged master SGP32 media-type registration; normal dispatch registration.
- !21672 — merged stable WSLua nil separator behavior; API backport.
- !21671 — merged stable GSM SIM file-identifier display improvement; protocol presentation.
- !21670 — merged stable WSLua column-append separator feature; API backport.
- !21669 — merged compiler/Coverity fixes; useful review clarified current ASTERIX generated-file boundary.
- !21668 — merged Debian dumpcap group rename to `_scap`; packaging/system integration.
- !21667 — merged AMS transaction tracking; representative capture, `_ret_uint` review, amend/squash workflow corroboration.
- !21666 — merged second CQLv5 support stage; protocol feature.
- !21665 — merged GitLab CI clang-version correction; CI maintenance.
- !21664 — merged DOF string-termination cherry-pick; same underlying safety lesson as !21651.
- !21663 — merged locale-stable Lua serialization and German-locale regression test; promoted.
- !21662 — merged Python cleanup in `check_typed_item_calls.py`; tooling maintenance.
- !21661 — merged automatic data/translation update; no durable review lesson.
- !21660 — merged automatic data/translation update; no durable review lesson.
- !21659 — merged automatic data/translation update; no durable review lesson.
- !21658 — merged automatic data/translation update; no durable review lesson.
- !21657 — merged stable IEEE 802.11 BIGTK display-name correction; no independent lesson.
- !21656 — merged another stable BIGTK display-name cherry-pick; no independent lesson.
- !21655 — merged stable IEEE 802.11 16-byte MME MIC support; protocol correctness/backport.
- !21654 — merged stable CFM ingress-interface alias off-by-one fix; no independent rule.
- !21653 — merged stable LLMNR transaction-conversation fix; promoted protocol-identity rule.
- !21652 — merged extcap probing verbosity reduction; operational/UI noise cleanup.
- !21651 — merged release-4.6 DOF string-termination safety fix by John Thacker; promoted produced-length rule.
- !21650 — merged CFM ingress-interface alias off-by-one cherry-pick; no independent rule.
- !21649 — merged Debian template wording backport; packaging/documentation maintenance.
- !21648 — merged DOF string-termination counterpart; same underlying fix as !21651.
- !21647 — merged GSM SIM identifier-display improvement; protocol presentation.
- !21646 — merged IEEE 802.11 BIGTK display-name correction; presentation fix.
- !21645 — merged IEEE 802.11 16-byte MME MIC support; protocol correctness.
- !21644 — merged stable SMB2 null-check-location backport; same mandatory-wire-consumption lesson as !21641.
- !21643 — merged WSLua nil-as-column-separator behavior; API usability.
- !21642 — merged master CFM linktrace ingress alias off-by-one correction; protocol correctness.
- !21641 — merged John Thacker SMB2 offset/null-check correctness fix; promoted mandatory wire-consumption rule.

## Weighting notes

Merged master changes were treated as the strongest implementation evidence. Stable backports were used to confirm that fixes were accepted for supported releases but were not counted as independent architectural votes for the same underlying patch. Open draft !21685 was explicitly down-weighted. No substantive Guy Harris review appeared in this batch, so no artificial Guy-weight was assigned. Direct review from John Thacker, Anders Broman, Alexis La Goutte, Gerald Combs, and other established maintainers was weighted according to specificity and whether the revised result merged.