# Wireshark MR review automation: !21491-!21540

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
Exact MRs reviewed in this run: 50

Before selecting this batch, the available per-run ledgers in `reviewed-mrs-automation/` and sparse tracking in `reviewed-mrs.md` were consulted as reviewed-set sources. The immediately preceding ledger ends at !21541, no previously reviewed sparse exception was found in !21491-!21540, and the historical !17571-!17620 batch remains preserved as part of the reviewed set. These are therefore the 50 highest-numbered previously unreviewed MRs present in the corpus at the commit above.

Weighting policy: merged master changes and substantive maintainer review carry the most weight; release backports primarily corroborate their master changes; closed/draft/superseded work is down-weighted and is not treated as accepted implementation precedent.

## Exact reviewed set

- !21540 — **Merged release backport / scanned.** `make-manuf.py` reports more specific HTTP/URL/generic exception reasons; backport of !21539.
- !21539 — **Merged master / scanned.** Improves `make-manuf.py` failure diagnostics by preserving actionable exception reasons. Useful tooling practice but no separate notebook rule promoted.
- !21538 — **Merged master / deep.** First-stage CQLv5 support stores negotiated protocol/version state in the conversation and switches framing after negotiation. Accepted by Anders Broman; protocol-specific state-machine evidence, but existing state/conversation guidance is sufficient.
- !21537 — **Merged release backport / scanned.** Stops fetching superseded minizip; dependency/build maintenance.
- !21536 — **Merged master / medium.** Adds Linux `mac80211_hwsim` Netlink dissection with create/query/delete testing. New dissector feature; no broadly additive review convention extracted.
- !21535 — **Merged / scanned.** Automated data/update maintenance; no durable new convention.
- !21534 — **Merged / scanned.** Automated data/update maintenance; no durable new convention.
- !21533 — **Merged release backport / scanned.** Automated data/update maintenance; no durable new convention.
- !21532 — **Merged release backport / scanned.** Automated data/update maintenance; no durable new convention.
- !21531 — **Merged / scanned.** Debian packaging adjustment after falcodump/libexec movement; packaging maintenance.
- !21530 — **Merged master / deep; promoted.** EAX moves mutable working state from function-static storage to the stack because it does not persist across calls, explicitly eliminating possible concurrency conflict. Promoted to `reentrancy-conventions.md`.
- !21529 — **Merged release backport / scanned.** Extcap install-RPATH follow-up for libexec location; backport of !21512.
- !21528 — **Merged release backport / scanned.** Increases 802.11 decryption buffer for standards-defined maximum MPDU size; backport of !21516.
- !21527 — **Merged release backport / scanned.** A-MSDU subframe-address offset correction; stable corroboration of !21511.
- !21526 — **Merged release backport / scanned.** A-MSDU subframe-address offset correction; stable corroboration of !21511.
- !21525 — **Merged release backport / scanned.** CQL global-spec parsing correction; backport of !21515.
- !21524 — **Merged master / scanned.** Stops CMake from fetching legacy minizip when minizip-ng is shipped; dependency maintenance.
- !21523 — **Merged / scanned.** Spelling/comment cleanup; no durable convention.
- !21522 — **Merged release backport / scanned.** Windows Kerberos dependency update; maintenance only.
- !21521 — **Closed draft / down-weighted.** RFC `tcpdiff` utility for matching TCP flows across two captures. Interesting architectural exploration around single-capture globals, but unmerged draft status makes it unsuitable as accepted precedent.
- !21520 — **Merged release branch / scanned.** Windows Kerberos dependency update; maintenance only.
- !21519 — **Merged release backport / scanned.** CMake Kerberos dependency update; maintenance only.
- !21518 — **Merged master / deep corroboration.** GTPv2 session identity is keyed above volatile UDP ports because ports can change while the logical session remains the same. Strong corroboration of `conversation-identity-conventions.md`; no duplicate rule added.
- !21517 — **Merged master / corroboration.** John Thacker Coverity fix frees a Wiretap buffer on the non-JSON-log failure path. Reinforces established acquired-resource cleanup guidance.
- !21516 — **Merged master / medium.** Raises dot11decrypt buffer size to the 802.11ac/ax/be maximum MPDU limit with standards references. Protocol-boundary maintenance; no new general rule.
- !21515 — **Merged master / deep.** Fixes CQL global-spec parsing: flags must reach the parser correctly and global keyspace/table metadata is consumed exactly once. Protocol-specific parser correctness; no separate durable rule needed.
- !21514 — **Merged master / scanned.** Fixes multiline C-comment matching in `check_spelling.py`; checker maintenance.
- !21513 — **Merged / scanned.** Typo cleanup including a few user-visible strings; no durable convention.
- !21512 — **Merged master / scanned.** Corrects extcap install RPATH following lib-to-libexec relocation; build/install maintenance.
- !21511 — **Merged master / medium.** Fixes an A-MSDU address computation by respecting that `msdu_offset` is relative to the A-MSDU rather than the enclosing MPDU. Good offset-coordinate-space correctness example, already covered by parser offset/boundary guidance.
- !21510 — **Merged master / deep; promoted.** Replaces a process-wide 128-KiB static Ericsson eNode-B parser line buffer with per-`wtap` private storage, reducing `.bss` and explicitly avoiding conflicts between concurrent parser instances. Promoted to `reentrancy-conventions.md` together with !21530.
- !21509 — **Merged master / scanned.** Removes Linux 2.1-and-older compatibility logic from executable-path discovery; obsolete-platform cleanup.
- !21508 — **Merged master / scanned.** CMake Kerberos dependency update; maintenance only.
- !21507 — **Merged master / strong corroboration.** John Thacker switches XML code from direct libxml2 structure-member access to a library function, with an explicit maximum limiting recursion/results. Reinforces stable-library-API and bounded-work guidance.
- !21506 — **Merged release backport / scanned.** Windows libssh dependency update; maintenance only.
- !21505 — **Merged release backport / scanned.** macOS libssh dependency update; maintenance only.
- !21504 — **Merged release backport / scanned.** macOS libssh dependency update; maintenance only.
- !21503 — **Merged master / scanned.** macOS libssh dependency update; maintenance only.
- !21502 — **Merged release backport / scanned.** libgcrypt dependency update; maintenance only.
- !21501 — **Merged master / scanned.** JSON Falco Events dispatch no longer falls back to the generic data dissector; protocol-dispatch correction without broader review lesson.
- !21500 — **Merged release backport / corroboration.** IAX2 validates address type before calling an API requiring a non-null/compatible address, resolving static-analysis warning; routine contract validation.
- !21499 — **Merged release backport / corroboration.** XML checks for NULL before `g_str_equal`, resolving static-analysis warning; routine contract validation.
- !21498 — **Merged release backport / corroboration.** RTPS NULL checks from static analysis; routine defensive correctness.
- !21497 — **Merged master / scanned.** macOS setup raises CMake baseline to satisfy newer Qt; build-tool lifecycle maintenance.
- !21496 — **Merged master / medium.** Packaging separates Stratoshark files from the main Wireshark package to avoid duplicate ownership. Packaging architecture, no broader notebook rule promoted.
- !21495 — **Merged master / deep corroboration.** Large public-header include cleanup with Michael Mann review on local-vs-installed include forms. Reinforces existing public-header/include-boundary guidance.
- !21494 — **Merged master / deep; promoted.** Fixes a protocol-tree field whose selected byte span, extracted width, signed formatting, and registered `hf_` type disagreed. Martin Mathieson explains that selecting more bytes than a field width risks truncation while an older protocol version legitimately selecting fewer bytes can still use a later-widened field. Promoted to `protocol-field-semantic-conventions.md`.
- !21493 — **Merged master / deep corroboration.** Bluetooth L2CAP control-field parsing arrives with a sample capture; Alexis La Goutte catches an uninitialized variable and Michael Mann repeatedly recommends `proto_tree_add_bitmask()` to model bitfields through normal field helpers. Reinforces representative-capture and semantic-helper guidance.
- !21492 — **Merged master / deep; promoted.** Adds a checker for suspicious `proto_tree_add_*` API choices. Martin Mathieson and Michael Mann explicitly push the tool toward diagnostic-by-default behavior rather than silent pre-commit rewriting, and separate broad semantic cleanup from the checker because correct fixes are not always mechanical. Promoted to new `checker-tooling-conventions.md`.
- !21491 — **Merged master / strong corroboration.** Michael Mann changes the RPC UI to use the generic SRT parameter mechanism rather than directly calling dissector APIs. Reinforces existing UI/application-to-dissector boundary guidance.

## Durable notebook changes from this run

1. **Mutable state belongs at the narrowest required lifetime — !21530 and !21510.** Added to `reentrancy-conventions.md`: use stack/local state for per-call scratch and instance-private state such as `wtap->priv` for per-reader lifetime; avoid accidental process-wide sharing through mutable `static` storage.
2. **Checker tools should diagnose by default, not silently rewrite — !21492.** Added in new `checker-tooling-conventions.md`. Rewriting must be explicit, and broad semantic fixes should not be bundled as mechanical transformations when the correct remedy can vary by field/context.
3. **Mechanically recognizable API misuse should become executable project policy — !21492.** Also added to `checker-tooling-conventions.md`, complementing existing structural-checker guidance.
4. **Field wire span, extraction width/domain, `hf_` type, and formatting must agree — !21494.** Added to `protocol-field-semantic-conventions.md`, including Martin Mathieson's nuance that a narrower old-version wire encoding may legitimately populate a widened semantic field while a wider extraction into a narrower field risks truncation.

## Strong corroboration retained without duplicate rules

- !21518 independently reinforces that conversation/session keys must represent the protocol's stable logical identity rather than mechanically using transport ports.
- !21507 reinforces preferring supported library APIs over direct dependency-internal structure access and bounding recursive/result work.
- !21495 reinforces installed/public-header include discipline and local include-path reasoning.
- !21493 reinforces representative capture submission and use of semantic proto-tree helpers such as bitmask APIs.
- !21491 reinforces keeping UI/application code on generic parameter/control interfaces instead of directly reaching into dissector APIs.
- !21517, !21500, !21499, and !21498 reinforce cleanup and library/API precondition checking surfaced by static analysis.
