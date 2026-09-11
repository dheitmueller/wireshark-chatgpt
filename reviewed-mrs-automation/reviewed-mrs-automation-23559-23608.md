# Wireshark MR Review Automation: !23559–!23608

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, aggregate tracking, and the individual ledgers under `reviewed-mrs-automation/`. Individual MR entries were used rather than treating a range filename or partial numeric interval as proof of coverage. The historical !17571–!17620 batch remains explicitly preserved and counts as reviewed. Intersecting the reconstructed reviewed set with the corpus at the commit above produced these fifty highest-numbered previously unreviewed MRs.

Exactly 50 MRs were reviewed, in descending order. The exact set is:

!23608, !23607, !23606, !23605, !23604, !23603, !23602, !23601, !23600, !23599,
!23598, !23597, !23596, !23595, !23594, !23593, !23592, !23591, !23590, !23589,
!23588, !23587, !23586, !23585, !23584, !23583, !23582, !23581, !23580, !23579,
!23578, !23577, !23576, !23575, !23574, !23573, !23572, !23571, !23570, !23569,
!23568, !23567, !23566, !23565, !23564, !23563, !23562, !23561, !23560, !23559.

## Review weighting and findings

Merged master work and substantive maintainer reasoning were weighted most heavily. Stable backports were used mainly as corroboration; closed/superseded work was down-weighted.

- **!23608 — Deep/promoted, merged master.** BLF closing logic pads/backpatches the final object so log containers remain 4-byte aligned. The change audits writable object framing, tests CAN/CAN FD/CAN XL/LIN/FlexRay/Ethernet combinations at adverse offsets, round-trips output, checks Vector interoperability, and benchmarks a 10-million-packet case. Promoted to `wiretap-writer-conventions.md`: padding included in an enclosing recorded length is safe only after proving every affected record has an unambiguous semantic payload boundary. !23657 from the previous batch corroborates the stable-branch acceptance.
- **!23607 — Scanned, merged.** libssh 0.11.4 dependency update; no general rule.
- **!23606 — Scanned, merged documentation.** Developer's Guide Git workflow cleanup/backport instructions; mostly reinforces existing submission guidance.
- **!23605 — Deep/corroboration, merged Coverity fix.** Adds defensive O-RAN beam-array bounds handling; existing bounds/static-analysis guidance covers it.
- **!23604 — Scanned, merged.** SMB bool/const cleanup; no distinct rule.
- **!23603 and !23599 — Deep/corroboration, merged macro cleanups.** Review caught accidental removal/change of runtime/user-visible dissector behavior while removing registration macros; fixes restored the behavior. Strongly reinforces the notebook's existing rule that cleanup/refactoring still requires semantic regression review.
- **!23602 — Scanned, merged.** WSDG Lua documentation reorganization.
- **!23601 and !23571 — Deep/corroboration, merged.** Use proto-tree `_ret_` helpers to avoid fetching the same packet value again. Existing fetch-once guidance already covers the pattern.
- **!23600 — Discussion-focused, merged.** QCDIAG log-code work raised reuse/centralization of shared definitions and led toward data-driven dictionaries; no separate new rule beyond existing helper/data ownership guidance.
- **!23598, !23594, !23588, !23586, !23582, !23580, !23572, !23565, !23563, !23559 — Deep/corroboration, merged.** Large family of unsigned-offset and status/out-parameter TVB search migrations. Counted individually but not duplicated in the notebook because parser API/type-domain guidance already covers them.
- **!23597 — Deep/promoted, merged release fix.** SOCKS direction handling under Decode As compares against `pinfo->match_uint` rather than a hard-coded default port and restores the actual matched context around nested dissection. Merged by Jaap Keuter and approved by Michael Mann. Promoted to `dissector-dispatch-context-conventions.md`.
- **!23596, !23595, !23592 — Scanned, merged master/backports.** Const-correctness adjustment for `wmem_strbuf_get_str()` under newer GCC; straightforward API correctness.
- **!23591 — Down-weighted, closed predecessor.** Contributor opened from the wrong branch and without the updated declaration; maintainer feedback required a feature branch and the work was resubmitted as merged !23592. Existing submission/topic-branch guidance already covers it.
- **!23590 — Discussion-focused, merged.** Commit-message hook now presents actionable standards when rejecting a commit; Martin Mathieson emphasized avoiding a workflow that encourages loss/retyping of a detailed commit message. Useful tooling evidence but not promoted beyond existing actionable-diagnostics guidance.
- **!23589 — Scanned, merged documentation.** Corrects a Developer's Guide Lua API example.
- **!23587 — Deep, merged.** QCDIAG log-command support and associated maintainer review; protocol-specific/data-model work without a new cross-project rule.
- **!23585, !23584, !23583, !23575, !23574, !23573 — Scanned, merged master/backports.** Diameter RAT-Type value/specification updates; standards data maintenance.
- **!23581 — Scanned, merged.** O-RAN tap/logging changes; feature-specific.
- **!23579 and !23577 — Scanned, merged master/backport.** Removes a duplicate semicolon; trivial correctness cleanup.
- **!23578 — Scanned, merged.** NSIS uninstall cleanup for global plugins; packaging-specific.
- **!23576 — Down-weighted, closed predecessor of !23577.** Superseded by the merged version; not used for policy extraction.
- **!23570 and !23569 — Deep/promoted, merged release/master; authored and merged by Guy Harris.** tshark distinguishes output failure from filtering results, checks `print_packet()`/`fflush()` explicitly, propagates a print-error state, and stops processing rather than continuing dissection after broken-pipe/filesystem/quota failure. Promoted to `cli-exit-status-conventions.md` with extremely high weight.
- **!23568 — Deep, merged.** DLMS/COSEM address parsing correction; CI/review also required squashing a trivial generated-file commit and clearing style findings. Submission details reinforce existing conventions.
- **!23567 — Scanned/discussion, merged.** SCTP DTLS key-management parameter support with ordinary spelling/review corrections; protocol-specific.
- **!23566 — Scanned, merged.** GSM SIM STATUS value added to Info column; presentation-specific.
- **!23564 and !23561 — Scanned, merged.** WiX schema/component fixes; Windows-installer-specific.
- **!23562 — Deep/corroboration, merged master; authored/merged by Michael Mann with Guy Harris and Jaap Keuter discussion.** Secret injection was refactored so epan-facing code needs only `wtap *`, not the larger `capture_file *`; Guy also distinguished Wireshark GUI `unsaved_changes` state from one-input/one-output tools such as tshark/editcap. Strongly reinforces existing dependency-layer and state-ownership guidance, so no duplicate notebook rule was added.
- **!23560 — Scanned, merged.** Developer checking tools handle Ctrl-C more cleanly; tooling usability improvement.

## Durable notebook promotion

- `wiretap-writer-conventions.md`: if an enclosing object length is backpatched to include alignment padding, prove every affected record type still has an unambiguous payload boundary; validate adverse alignments, heterogeneous record types, and independent/round-trip interoperability where possible (!23608; merged master, corroborated by !23657).
- `dissector-dispatch-context-conventions.md`: Decode-As-capable dissectors must base selector-dependent behavior on the actual dispatch key (`pinfo->match_uint`/equivalent), not a hard-coded default port, and must preserve/restore the real nested dispatch context (!23597; merged, Jaap Keuter/Michael Mann acceptance).
- `cli-exit-status-conventions.md`: output failure during packet processing is a first-class terminal result; explicitly detect, propagate, and stop on failed printing/flushing rather than continuing expensive dissection with no viable output sink (!23569 master and !23570 backport, authored and merged by Guy Harris).

Closed !23591 and !23576 were explicitly down-weighted. Repeated unsigned-offset migrations, fetch-once cleanups, protocol-data updates, backports, dependency maintenance, documentation-only changes, and installer-specific fixes were all counted in the exact reviewed set without creating duplicate notebook rules.