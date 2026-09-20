# Wireshark MR automation review: !15748 through !15699

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook starting HEAD: `9c13c2aba2a2bdbcfff44972d72f014d737491e9`

## Selection method

Before selecting this batch, review tracking was reconciled from `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the complete set of per-run ledgers under `reviewed-mrs-automation/`. The historical !17571-!17620 batch was explicitly preserved and counted as 50 already-reviewed MRs.

The immediately preceding exact ledger records !15798 through !15749. Searches across the notebook/review tracking for the candidate prefixes below !15749 found no previously reviewed candidate in this batch. Corpus enumeration confirmed that the fifty highest-numbered unreviewed MRs are exactly !15748 through !15699 inclusive. No numeric range was assumed reviewed merely from a ledger filename; selection was based on the tracked MR set.

## Exact reviewed set (50)

- !15748
- !15747
- !15746
- !15745
- !15744
- !15743
- !15742
- !15741
- !15740
- !15739
- !15738
- !15737
- !15736
- !15735
- !15734
- !15733
- !15732
- !15731
- !15730
- !15729
- !15728
- !15727
- !15726
- !15725
- !15724
- !15723
- !15722
- !15721
- !15720
- !15719
- !15718
- !15717
- !15716
- !15715
- !15714
- !15713
- !15712
- !15711
- !15710
- !15709
- !15708
- !15707
- !15706
- !15705
- !15704
- !15703
- !15702
- !15701
- !15700
- !15699

## Review weighting

All fifty corpus records were scanned, with discussions/diffs read more deeply where they contained review or architecture signal. Merged master changes were treated as the strongest implementation evidence. Closed/superseded proposals were retained in the audit set but down-weighted: !15734 was an earlier Matter BTP submission superseded by merged !15736, and !15738's single-flag MySQL midstream approach was explicitly superseded by merged !15741 after John Thacker's review. Release backports, automated data updates, version bumps, documentation-only changes, and straightforward mechanical cleanups were reviewed but not promoted unless they added a distinct durable rule.

## Durable findings promoted to the notebook

- **!15706, !15715, !15727 — fixed-array element counts.** Wireshark prefers the common `array_length()` helper over open-coded `sizeof(array) / sizeof(type)` for true compile-time arrays, while semantically different size/unit divisions must not be mechanically rewritten. !15715 also demonstrates that the cleanup can expose an actual boundary-value bug rather than being merely stylistic. Added to `c-array-size-conventions.md` in commit `c1c889045f93ab2fc51a69507c982c7ea2d32684`.
- **!15712 — semantic enum/type domains.** Conversation APIs should receive `CONVERSATION_*` values rather than numerically equivalent `ENDPOINT_*` values even when there is no current runtime difference; semantic-domain correctness protects future divergence and documents the API contract. Added to `api-domain-conventions.md` in commit `92b24baa25ed5f6b8535c103c5015dfd7401bd6b`.
- **!15705 — shared dissector-data initialization.** John Thacker's merged CAN-ETH fix shows that when a context structure such as `can_info` gains fields, every producer must initialize the complete downstream-visible contract before calling shared subdissectors. Added to `dissector-context-flow-conventions.md` in commit `23c9c007d7c634291328ef4c6b0d85ee0bcba34e`.
- **!15738 / !15741 — coherent state for captures that start midstream.** The closed proposal guessed one MySQL capability flag when login was absent; John Thacker questioned the inconsistency across other capability checks, and the merged replacement establishes a minimal default capability set centrally. Added to `state-representation-conventions.md` in commit `43b46e4f8620c1d555d12b1fbadf48f19c051a0b`.
- **!15745 — reload ownership.** John Thacker's merged WSLua fix unreferences existing static `GPtrArray` / `GHashTable` containers before replacing them during plugin re-registration. Reloadable initialization must treat the second and later run as an ownership transition rather than as another first initialization. Added as `reload-lifecycle-conventions.md` in commit `8f661e4235dd6d9de4a6f29c28a396664d73f18d`.
- **!15730, with !15727 context — CI matrix limits for broad refactors.** A green Fedora MinGW job did not exercise AirPcap, and the same broad header/array helper work also broke Falco and Debian packaging. John Thacker explicitly noted the practical CI-minute limit on building every platform/configuration combination. Added as `ci-build-matrix-conventions.md` in commit `2eb19848dfa978b85dbf23d5c938ed2df62be9c5`.

## Useful corroboration retained without duplicating rules

- !15723 is valuable crash/debugging history for the SPRT redissection problem, but the later merged !15786 already supplies the stronger root-cause convention that a revisited frame can take a different protocol path and therefore lack protocol-private state.
- !15708 reinforces the existing rule that a header-field blurb should add information beyond the label rather than simply repeat it; no duplicate rule was added.
- !15733 fixes sharkd `stream`/`sub_stream` output by making fields conditional on the Follow Stream implementation actually supporting/providing those values, avoiding uninitialized output. It is a release backport and useful supporting API evidence, but no separate notebook rule was necessary in this run.
- !15704 adds a regression case for an empty indefinite-length CBOR byte string and avoids finalizing an empty composite tvbuff; useful parser-edge coverage, but it is a stable-branch cherry-pick and does not justify a duplicate general empty-input rule.
- !15714 includes maintainer enforcement of the pre-commit checker on a protocol feature; this corroborates the established submission/checker workflow.
- !15734 was closed because the contributor submitted from a protected `master` source branch and then resubmitted from a topic branch; it corroborates existing clean/topic-branch submission practice but, as an abandoned submission, was not promoted over merged evidence.
- !15699 continues the accepted migration from GLib scalar aliases to standard C/C99 types inside Wireshark-owned code; this is already represented by stronger prior type-convention evidence.

This ledger is the authoritative exact record for this automation run.