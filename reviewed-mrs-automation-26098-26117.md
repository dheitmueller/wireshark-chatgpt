# Supplemental Reviewed Wireshark Merge Requests — !26098–!26117

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit used for this batch: `2261458afc3e287c111ffe13f534d9aaf5af07af`

Exactly twenty MRs were reviewed in this batch: **!26098 through !26117 inclusive**.

| MR | State / target | Review depth | Durable result / weighting |
|---|---|---|---|
| !26098 | merged, master | Deep | MCTP Control payload expansion adds command-specific decoding, reserved-bit visibility, malformed/truncation handling, a synthetic 27-frame capture, 16 focused tests, adversarial cases, and bounded fuzzing. Anders Broman approved/merged. Strong testing and dissector-hardening exemplar; existing notebook testing guidance already covers most reusable lessons, so retained chiefly as corroboration. |
| !26099 | merged, master | Deep | Ronnie Sahlberg fixes LDAP ETW parsing that treated an explicit-length property buffer as a NUL-terminated, minimum-width string and decoded back into the same undersized allocation. Accepted fix length-checks every access, uses bounded comparison and explicit hex decoding, and writes to a separately bounded buffer. Promoted to `parser-edge-cases.md`. Ronnie also judged the obscure, user-assisted ETL path insufficient to justify stable-branch backports; that release-severity judgment is retained here rather than generalized. |
| !26100 | merged, master | Scanned | BTLE peripheral session IV field corrected from `FT_UINT64` to the specified 32-bit width. Straight protocol-field correctness fix with Anders Broman approval; no distinct notebook rule needed. |
| !26101 | merged, release-4.4 | Scanned / generated update | Automatic enterprise/manufacturer/services data update for the 4.4 branch. Accepted routine maintenance, but not independent coding/review evidence. |
| !26102 | merged, release-4.6 | Scanned / generated update | Automatic registry/data update for the 4.6 branch. Accepted routine maintenance, but not independent coding/review evidence. |
| !26103 | merged, master | Scanned / generated update | Master-branch automatic registry/manufacturer/translation data update. Gerald Combs authored and Anders Broman merged; routine generated-data maintenance, not promoted as a convention. |
| !26104 | merged, master | Deep / high authority | Authored and merged by John Thacker. AVRCP reassembly sums fragment lengths with checked arithmetic, caps the implementation domain at `INT32_MAX`, and clips copies to remaining allocated capacity while flagging overlong reassembly. Strong independent corroboration of the aggregate-reassembly rule already promoted from !25844. The code also notes that the standard reassembly API would better handle dependent frames, reinforcing the notebook's preference for established reassembly infrastructure without creating a new rule. |
| !26105 | merged, master | Deep / high authority | Authored and merged by John Thacker. DCT3 wiretap parser stops after the schema's single allowed `<l2>` child, strengthens an output-end guard from equality to `>=`, and routes an error through common cleanup so the XML document is freed. Promoted to `parser-edge-cases.md` as structural multiplicity/output-bound/cleanup guidance. |
| !26106 | merged, release-4.6 | Backport / down-weighted | Coverity-driven AVRCP null check. John Thacker explicitly noted the lookup may not actually be nullable in practice but accepted the defensive check while the file was already being changed. Useful static-analysis hygiene, but weak evidence for a new semantic rule and only a release backport. |
| !26107 | merged, release-4.6 | Backport/corroboration | Backport of the DCT3 parser fix in !26105. Corroborates the promoted structured-parser safety rule; not counted as independent evidence. |
| !26108 | merged, release-4.6 | Backport/corroboration | Backport of the AVRCP aggregate-reassembly overflow fix in !26104. Corroboration only. |
| !26109 | merged, release-4.4 | Backport/corroboration | Second release backport of the Coverity AVRCP null check from !26106. Corroboration only. |
| !26110 | merged, release-4.6 | Backport/corroboration | UMTS RRC bounds-checks both packet-derived indices before writing `seq_no[rbid][direction]`. This is a backport of the master fix already reviewed as !26072 and therefore only corroborates the existing multidimensional-array bounds rule. |
| !26111 | merged, release-4.4 | Backport/corroboration | Second stable-branch backport of the UMTS RRC fix already promoted from !26072. Corroboration only. |
| !26112 | merged, release-4.4 | Backport/corroboration | 4.4 backport of the AVRCP aggregate-reassembly overflow fix in !26104. Corroboration only. |
| !26113 | merged, master | Medium / high authority | Authored and merged by Stig Bjørlykke. GSM SIM SELECT P2 decoding separates the RFU high bit from the return-data mask and corrects session-control values. Strong protocol-bitfield correctness evidence and corroboration of the existing rule to expose reserved/RFU bits rather than folding them into another semantic field. |
| !26114 | merged, master | Deep | Compact JSON output feature received explicit architecture feedback from John Thacker to place the option in shared `print_args_t` so GUI and CLI can use the same formatter contract; final implementation does so and exposes the GUI checkbox. Martin Mathieson also pointed to existing `print_indent()` performance infrastructure. Promoted to `platform-gui-conventions.md`. |
| !26115 | merged, release-4.6 | Backport/corroboration | Backport of the GSM SIM P2/RFU fix in !26113. Corroboration only. |
| !26116 | merged, master | Medium | IEEE 1722 ACF-CAN fixes parser cursor advancement so padding begins after the decoded payload rather than at the stale pre-payload offset. First contribution, approved/merged by Anders Broman. Correct offset-accounting example, but existing parser-cursor guidance is sufficient; no new rule promoted. |
| !26117 | merged, master | Medium / high authority | John Thacker removes obsolete MaxMindDB CMake probe scaffolding that no longer performed any check; Gerald Combs approved/merged. Promoted to `build-conventions.md`: remove state/setup for deleted feature tests so find modules describe actual checks rather than historical no-ops. |

## Weighting notes

Merged master-branch MRs received the greatest weight. Stable-branch MRs !26101, !26102, and !26106–!26112 plus !26115 were treated as routine branch maintenance or corroboration; repeated backports do not multiply the evidentiary weight of the underlying fix.

!26104 and !26105 receive particularly strong weight because John Thacker both authored and merged the master fixes and their code states the safety rationale directly. !26114 receives strong review weight because John Thacker's frontend-architecture request was incorporated before merge and Martin Mathieson supplied implementation-performance context. !26113 is also strong maintainer evidence from Stig Bjørlykke. Anders Broman's approvals/merges were weighted as acceptance evidence throughout.

No abandoned/open draft in this twenty-MR range was promoted over a merged implementation. Generated registry updates were reviewed and recorded to keep the ledger exact, but were not mined for general coding rules.

## Notebook changes from this batch

- `parser-edge-cases.md`: explicit-length external buffers are not C strings; enforce structured-input multiplicity/output bounds in control flow and keep resource cleanup on error paths.
- `platform-gui-conventions.md`: cross-frontend output/export options belong in shared frontend-independent print/configuration state; reuse existing common formatter helpers before adding frontend-local performance paths.
- `build-conventions.md`: delete obsolete configure/probe scaffolding when the capability test itself no longer exists.
- Existing aggregate-reassembly overflow, multidimensional bounds, reserved-bit visibility, testing/fuzzing, and parser-offset guidance was corroborated but not duplicated.

Notebook commits created before this ledger:

- `744e3b3530e56f9288a5b8f2dad54d3bec77dd3e` — explicit-length and structured-parser safety conventions.
- `b3d303a8e702dd595b9694aab9c0f3fa72d03d9e` — shared CLI/GUI print-option convention.
- `17745c408dffecb0af954cfa6ff53c607fef8d5a` — stale CMake probe cleanup convention.
