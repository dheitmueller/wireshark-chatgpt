# Wireshark MR automation review: !13113 through !13162

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This ledger records the exact set completed by this run. Selection was reconciled from the available `reviewed-mrs.md` and the per-run tracking under `reviewed-mrs-automation/`; it was not inferred from numeric range filenames. The prior !13163-!13212 ledger explicitly recorded !13162 only as a frontier check, so it remained eligible here. The historical !17571-!17620 batch remains part of the already-reviewed set.

Merged/accepted outcomes were weighted more heavily than abandoned, failed-backport, or superseded work. Maintainer review was weighted by authority, including direct Guy Harris guidance where it materially clarified architecture or semantics.

## Exact reviewed MR set

!13162, !13161, !13160, !13159, !13158, !13157, !13156, !13155, !13154, !13153, !13152, !13151, !13150, !13149, !13148, !13147, !13146, !13145, !13144, !13143, !13142, !13141, !13140, !13139, !13138, !13137, !13136, !13135, !13134, !13133, !13132, !13131, !13130, !13129, !13128, !13127, !13126, !13125, !13124, !13123, !13122, !13121, !13120, !13119, !13118, !13117, !13116, !13115, !13114, !13113

Count: **50**.

## Durable findings promoted

- **!13144 — Deep, merged master, João Valverde.** Display-filter tokens can be lexically ambiguous: a spelling such as `fc` can denote a protocol/field or a byte literal, and dotted strings can likewise have more than one possible interpretation. Earlier attempts to force the decision during lexing produced corner cases and weaker diagnostics. Promoted to `parser-phase-separation-conventions.md`: preserve an explicit unresolved/unparsed representation when semantic context is required, and resolve it during type/semantic checking rather than guessing in the lexer. Notebook commit: `154a33d75532abe569c8ad9e717e5164babb4a0c`.
- **!13129 — Deep, merged master, John Thacker.** TCP PDU export previously could publish bytes that a subdissector had requested be desegmented and would therefore be presented again after reassembly. Promoted to `stream-export-conventions.md`: export/tap only the prefix actually consumed on the current pass; suppress export when `desegment_offset == 0`, and audit all stream-dispatch paths for the same consumed-byte semantics. This is particularly important for first-pass and one-pass TShark behavior. Notebook commit: `5d98bc28c714b1cf08b07ea341dac8d6f01783a0`.
- **!13122 — Deep, merged master, João Valverde, with direct Guy Harris architectural review.** Saved filter lists were changed from static/eager state to dynamic on-demand reads. Guy explicitly considered same-process edits, profile changes, and another running Wireshark instance, and observed that if complete invalidation notification would require ugly cross-component plumbing, rereading a small file when needed is probably the right answer. Promoted to `configuration-reread-conventions.md`: include invalidation complexity and externally observable mutation sources in the cache decision; for cheap mutable configuration, a fresh snapshot can be safer than a long-lived cache. Notebook commit: `a977b9efb4a3b082b30ee1f399a2580367c7c575`.
- **!13116 — Deep, merged master; !13139 — merged release backport.** `sharkd` had lost the state needed to compute `frame.time_delta_displayed` relative to the preceding *displayed* frame, so a sparse result containing frames 1 and 800 incorrectly used frame 799. The accepted fix restores displayed-frame/reference bookkeeping and adds a regression test; Guy Harris approved the master change. Promoted to `displayed-frame-context-conventions.md`: view-relative fields must carry view-relative predecessor/reference state in every frontend, with sparse-filter tests separating capture order from display order. Notebook commit: `95172e1554207c3944fa473667097ea4e3a1f2eb`.

## Strong corroboration and notable review evidence intentionally not duplicated

- **!13141 — merged master.** The Qt display-macro editor adds a validator matching the macro-name lexical contract (letters, numbers, underscore). This corroborates later notebook guidance that UI validation and parser token rules should agree, without requiring a duplicate convention.
- **!13115 — merged master.** Removes a redundant display-filter function-name assertion because registration already owns the validation. This corroborates the existing shared-boundary/precondition rule: validate once at the authoritative operation boundary rather than duplicating checks at every caller.
- **!13123 — merged master.** Centralizes `dftest` invocation construction in a pytest fixture, useful test-harness consistency evidence but not broad enough to promote separately.
- **!13137 — merged master.** Removes PINOs from the Enabled Protocols dialog because they are controlled by their parent and cannot be toggled independently. Useful UI/model truthfulness evidence: do not expose an independent control for state the underlying model cannot independently change.
- **!13136 — merged master, Peter Wu.** Expands the TLS Keylog Launcher from a browser path to a real command line parsed for direct process launching, with discussion around quoting, environment expansion, and avoiding unnecessary shell mediation. Retained as process-launch/UI evidence rather than generalized beyond the accepted implementation.
- **!13156 and related stable backports — merged.** GSM SIM READ/UPDATE BINARY parsing correctly uses the command mode bit to decide whether P1/P2 is a binary offset or contains an SFI. Protocol-specific evidence that discriminator bits must govern interpretation before fields are decoded.
- **!13150 — merged release backport.** Corrects ISO-TP STmin values 0xF1-0xF9 to 100-900 microseconds rather than milliseconds. Useful unit/representation correctness evidence, but protocol-specific.
- **!13140 — merged master.** Repairs copy/pasted display-filter abbreviations in several dissectors, reinforcing existing registration/checker hygiene rather than creating a new rule.
- **!13145 / !13162 — merged master, Gerald Combs.** CI makes generated Debian symbols downloadable and then publishes the Debian package artifacts even when the job fails. Useful diagnostics/reproducibility evidence, but no additional notebook convention was needed in this batch.

## Closed/unmerged work down-weighted

- **!13151 — closed release-4.2 backport.** John Thacker noted that the Ethernet/PRP trailer change depended on several smaller trailer commits to cherry-pick cleanly and was comfortable leaving it out of 4.2. The underlying master-side work may still be useful, but this failed/abandoned backport is not treated as independent accepted design evidence.
- **!13130 — closed draft.** The file-dialog home-directory fallback direction had useful discussion, including Guy Harris's cross-platform policy concerns, but the proposal's control flow meant the intended branch was not reached under the normal default. Later merged work already supplies stronger notebook evidence for file-dialog/history policy.
- **!13121 — closed/unmerged.** Proposed TCP reassembly annotation changes contained an unrelated iSCSI commit and John Thacker identified a regression if `first_pdu` was not tested. It is retained as negative review evidence only; later merged !13397 supplies the accepted reassembly-presentation rule.

The remaining MRs in the exact set were examined for diffs, discussions, merge outcome, and reusable review evidence. They consist mainly of accepted protocol-specific fixes, UI/style work, stable-branch backports, generated/dissector data changes, and small test/tooling corrections that did not justify additional durable notebook guidance beyond the promoted or corroborating items above.

## Frontier

!13112 (`epan: Convert uat.h indentation to 4-space`) was inspected only as a frontier check after this batch. It exists in the same corpus commit and is merged, but it is **not** part of this reviewed set and must remain eligible for the next descending batch.
