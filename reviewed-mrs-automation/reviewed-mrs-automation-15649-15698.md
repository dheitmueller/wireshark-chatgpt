# Wireshark MR automation review: !15698 through !15649

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook starting HEAD: `2384dd7ba5d8682e13a8e9b9e3cc21adc8b1ec8b`

## Selection method

Rebuilt the already-reviewed set from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the complete set of per-run files under `reviewed-mrs-automation/`. The historical `reviewed-mrs-automation/17571-17620.md` batch remains explicitly preserved and counted as 50 reviewed MRs. Selection was done by exact set subtraction against the corpus, not by assuming that a numeric interval was reviewed because neighboring MRs appeared in a ledger.

The immediately preceding authoritative ledger covers !15748 through !15699. Repository-wide review-tracking searches found no already-reviewed !156xx MRs below !15699, while the corpus contains every MR from !15698 through !15649. Therefore these are the fifty highest-numbered corpus MRs not already reviewed.

## Exact reviewed set (50)

- !15698
- !15697
- !15696
- !15695
- !15694
- !15693
- !15692
- !15691
- !15690
- !15689
- !15688
- !15687
- !15686
- !15685
- !15684
- !15683
- !15682
- !15681
- !15680
- !15679
- !15678
- !15677
- !15676
- !15675
- !15674
- !15673
- !15672
- !15671
- !15670
- !15669
- !15668
- !15667
- !15666
- !15665
- !15664
- !15663
- !15662
- !15661
- !15660
- !15659
- !15658
- !15657
- !15656
- !15655
- !15654
- !15653
- !15652
- !15651
- !15650
- !15649

## Review weighting

Merged master changes were treated as the strongest evidence. Stable-branch backports were used primarily as corroboration for fixes already accepted on master. The closed draft !15695 (`Investigate cppcheck`) was down-weighted and not treated as accepted project direction. Release/version/build automation MRs and low-discussion mechanical changes were scanned for conflicts and useful submission/tooling evidence but were not promoted merely because they merged.

Maintainer authority was applied contextually. Guy Harris's discussion on !15689 received exceptional architectural weight: he explicitly framed the maintenance/performance tradeoff between calling an optimized external zlib implementation and vendoring a faster CRC implementation. Because the final accepted outcome left both strategies viable rather than establishing one as project policy, this was retained as review evidence rather than over-generalized into a notebook rule. Gerald Combs's authored-and-merged CI correction in !15659 was treated as exceptionally authoritative project-wide CI evidence. John Thacker-authored merged correctness changes received high weight, especially where their rationale defined API, lifetime, or representation contracts.

## Durable findings promoted

- **!15671 — merged master / very high weight.** MySQL result-set strings can have a little-endian length prefix while the negotiated text encoding can be big-endian UTF-16/UTF-32. A composite `FT_UINT_STRING` cannot express those independent encoding rules, so the accepted implementation splits the length and string fields. Added `wire-encoding-conventions.md`.
- **!15669 — merged master / very high weight.** UDP payload output must remain beneath the tree context in which UDP itself was dissected; attaching payload subtrees to the packet root breaks structural hierarchy for nested/quoted packets such as ICMP errors. Anders Broman questioned the tunnel/error-packet implications before merging the clarified fix. Added `protocol-tree-hierarchy-conventions.md`.
- **!15667 — merged master / high weight.** SMB2 state changed its invalid request/response-frame marker to `UINT32_MAX` because zero can be a valid frame value. Added an explicit rule to `state-representation-conventions.md` that invalid-state sentinels must lie outside the legitimate value domain.
- **!15659 — merged master / extremely high weight.** Gerald Combs corrected an MR-scoped GitLab API query to use `CI_MERGE_REQUEST_PROJECT_ID` rather than generic `CI_PROJECT_ID`. Added `ci-merge-request-context-conventions.md` covering the distinction between execution-project and merge-request identity, especially for fork-originated pipelines.
- **!15684 — merged stable backport / very high corroborating weight.** Plugin teardown must occur after wmem scope destruction because allocator callbacks can still call into plugin code. Added `plugin-lifecycle-conventions.md`; the backport points to the already merged master fix.
- **!15660 — merged stable backport authored and merged by John Thacker / very high corroborating weight.** Reusing an initial DSB aggregate across output-file rotation requires preserving ownership of both the `GArray` and each contained refcounted block. Added `reference-counted-aggregate-conventions.md`.

## Strong corroborating evidence retained without duplicating rules

- **!15697 and !15696:** TCP analysis/graphing preserves the right raw/relative sequence representation and routes fragmented-header sequence values through the canonical analysis path, reinforcing existing sequence-number representation guidance.
- **!15688:** John Thacker's Zigbee GP fix refuses to decrypt a truncated tvbuff when the crypto length derives from reported length but the allocated bytes derive from captured length, strongly corroborating existing captured-vs-reported-length and truncation rules.
- **!15682:** the Time Shift dialog backport makes the redissection signal a `Qt::QueuedConnection` so a `DeleteOnClose` dialog cannot resume after synchronous redissection and touch widgets already destroyed by user action; this corroborates the existing asynchronous-completion lifetime guidance.
- **!15658:** John Thacker's WSLua fix isolates per-call Lua execution state so a Wireshark `longjmp` cannot leave stale Lua jump-buffer state. The notebook already contains the stronger later !21759 version of the same exception-boundary rule, so no duplicate rule was added.
- **!15650:** Martin Mathieson's review explicitly requested either a shareable capture or fuzzing for the large DOCSIS 4.0 dissector change, and accepted fuzzing as useful validation when NDA constraints made a capture difficult to share. This corroborates existing test/fuzzing guidance rather than creating a new rule.
- **!15649:** the accepted DTLS 1.3 series includes updating the next sequence number only after successful decryption, useful corroboration for commit-after-success state transitions in security-sensitive parsing.
- **!15663 with !15665:** the master CMake fix for versioned Lua library paths was immediately carried to release-4.2, providing build-system portability/backport evidence but no new general rule beyond existing dependency-discovery guidance.

## Notebook commits made by this run

- `58d1a27518173e32d62a995c29bd74b537f3f604` — document invalid-state sentinel convention from !15667.
- `d43e5ded7d1e8e53cfbb0d003124ce1ab252fce8` — add independent composite wire-encoding convention from !15671.
- `64e9ccc90950f3004e98fa2b969c52b5dc076069` — add protocol-tree hierarchy convention from !15669.
- `717a348506103d4487a2537643bbedab29a9cfc6` — add merge-request CI identity convention from !15659.
- `d71486dc06776c93466ec65f986a79c0f7d7aeef` — add plugin callback teardown ordering from !15684.
- `20c280fa83a383b622f3913440f4628df8bd6d95` — add reference-counted aggregate ownership convention from !15660.

This file is the authoritative exact per-run ledger for these 50 reviewed MRs. `reviewed-mrs.md` was consulted but not rewritten because the per-run ledger is sufficient to preserve exact reviewed-set correctness.
