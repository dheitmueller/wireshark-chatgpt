# Automated MR review: !17309-!17358

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest toward older MRs. Existing tracking in `reviewed-mrs-automation/` and `reviewed-mrs.md` was consulted before selection. The previously reviewed !17571-!17620 batch remains counted. The fifty highest-numbered corpus MRs not represented by existing tracking are !17358 through !17309.

## Exact reviewed set

!17358, !17357, !17356, !17355, !17354, !17353, !17352, !17351, !17350, !17349, !17348, !17347, !17346, !17345, !17344, !17343, !17342, !17341, !17340, !17339, !17338, !17337, !17336, !17335, !17334, !17333, !17332, !17331, !17330, !17329, !17328, !17327, !17326, !17325, !17324, !17323, !17322, !17321, !17320, !17319, !17318, !17317, !17316, !17315, !17314, !17313, !17312, !17311, !17310, !17309.

Count: 50.

## Review weighting and durable observations

Merged master changes were weighted most heavily. Release-branch cherry-picks were treated mainly as corroboration, and abandoned/superseded changes as contextual or negative evidence.

- **!17358 (merged, master; John Thacker)** removes `G_REGEX_OPTIMIZE` from a regex compiled for one-shot use. The option cannot amortize its optimization cost in that lifecycle and was implicated in a GLib/PCRE2 partial-match uninitialized-memory problem. Gerald Combs also requested tightening the regex itself; John incorporated the review before merge. Useful accepted evidence for matching optimization choices to object lifetime/use frequency and for tightening parsers when the accepted input grammar is known.
- **!17354 (merged)** sets GOOSE's actual packet length because it is Ethernet-carried, allowing the enclosing Ethernet dissector to correctly reason about FCS/trailer bytes. This reinforces existing guidance that child dissectors should preserve framing/length semantics required by their parent encapsulation.
- **!17350 (merged, master)** adds three IEEE 1905/EasyMesh MLD configuration TLVs and supplies focused sample pcaps/screenshots for each form. This is further corroboration of the existing sample-capture expectation for protocol additions.
- **!17340 (merged, release-4.4; John Thacker)** installs the newly available AMR-NB codec plugin in both Windows packaging systems. As a backport with no substantive review, it is corroborative packaging evidence only.
- **!17330 (merged, master)** updates IEEE 1905 media types and frequency bands directly from the IEEE 1905/EasyMesh specifications, including correcting 802.11ax semantics and adding 802.11be/6 GHz. Useful protocol-registry/specification maintenance evidence, but already covered by notebook guidance to anchor protocol constants in authoritative registries/specifications.
- **!17324 (merged)** explicitly initializes SMB2 `disconnect_frame` state to zero, preventing stale/uninitialized state from influencing later analysis. This reinforces existing initialization/state-lifetime guidance rather than adding a new rule.
- **!17320 (merged, master)** enables TLS exporter-secret injection after the corresponding consumer support was introduced. Review explicitly sought TLS-domain expertise before merge. Useful corroboration that security/protocol changes should be routed to maintainers with the relevant domain knowledge when semantics are specialized.
- **!17310 (merged, release-4.4; Jaap Keuter)** asserts that IRDMA endpoint analysis data exists because the remainder of the path fundamentally requires it. This is a backport and therefore secondary evidence, but it reinforces distinguishing internal invariant failures (assert) from malformed packet input that should be reported/handled normally.

The remainder of the batch was scanned for merge outcome, purpose, substantive discussion, and final change context. Routine backports, generated/data updates, localized dissector fixes, and changes without reusable human-review evidence were not promoted into general notebook rules.

## Notebook action

No convention file was changed in this run. The useful findings reinforce existing guidance on parent/child framing semantics, sample captures for protocol work, specification-backed constants, state initialization, domain-expert review, and assertions for internal invariants. This run ledger is the notebook update.

## Continuation

Rebuild the reviewed set from all tracking before the next run. If no newer gaps have appeared, the next descending candidate after this batch is !17308.
