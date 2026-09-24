# Wireshark MR automation review: !11112 through !11063

Reviewed on 2026-09-24 with GPT-5.6 Sol.

## Corpus and selection

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Direction: descending MR number, newest available unreviewed MRs first.
- Before selecting the batch, the reviewed set was rebuilt from the available notebook tracking, including `reviewed-mrs.md` and the per-run files in `reviewed-mrs-automation/`. The historical !17571-!17620 batch was preserved and counted.
- Selection was performed by exact MR-number membership in the rebuilt reviewed set, not by assuming that a numeric interval was already reviewed because some entries in it appeared in a ledger.
- The prior mention of !11112 was only a frontier probe, not a completed review. !11063 was likewise explicitly described in earlier notebook material as a not-yet-reviewed master MR.
- Exactly 50 MRs were reviewed in this run. All 50 were merged in the corpus snapshot.

## Exact reviewed MR set

!11112, !11111, !11110, !11109, !11108, !11107, !11106, !11105, !11104, !11103,
!11102, !11101, !11100, !11099, !11098, !11097, !11096, !11095, !11094, !11093,
!11092, !11091, !11090, !11089, !11088, !11087, !11086, !11085, !11084, !11083,
!11082, !11081, !11080, !11079, !11078, !11077, !11076, !11075, !11074, !11073,
!11072, !11071, !11070, !11069, !11068, !11067, !11066, !11065, !11064, !11063.

State summary: **50 merged; 0 closed/unmerged; 0 open**.

## Durable findings and weighting notes

- **!11063 — deep, merged master, John Thacker authored/merged.** TCP out-of-order reassembly must re-evaluate fragments already retained beyond a gap when newly contiguous data closes that gap; otherwise the contiguous frontier can remain stale and higher-layer PDUs can be missed. The MR adds a dedicated capture and exercises the path both normally and with `tshark -2`. The previously reviewed release backport !11114 corroborates the importance of the fix. Promoted to `reassembly-contiguous-frontier-conventions.md`.
- **!11102 — deep, merged master, John Thacker authored/merged.** Optimizes that same frontier calculation by starting from the reassembly framework's first known gap and by recomputing only after mutations that can change the contiguous region. This avoids repeatedly performing linear fragment scans inside the out-of-order processing loop. Promoted with !11063.
- **!11098 — deep, merged master, Guy Harris authored/approved/merged.** Bounded formatting must distinguish an upper-bound length that includes the terminating NUL from the actual non-NUL text length and from destination capacity. Guy's rewrite also bases empty-address behavior on the formatted representation being empty, rather than on one special address type. Extremely high-authority evidence; promoted to `buffer-formatting-capacity-conventions.md`.
- **!11079 — deep, merged master, John Thacker authored/merged.** QUIC header-protected fields are displayed only after deprotection, so the accepted code marks the decoded tree items generated rather than implying that the shown values literally occupy the cited protected bytes. Promoted to `transformed-field-presentation-conventions.md`; merged !11089 independently corroborates the same distinction for a derived absolute PFCP validity timestamp.
- **!11092 — deep, merged master, approved/merged by Alexis La Goutte.** Heuristic recognition should use cheap protocol invariants such as reserved bytes to reduce false positives, but should not accumulate every possible validity check because over-strict recognition hides malformed protocol traffic from the dissector. Promoted to `heuristic-recognition-balance-conventions.md`.
- **!11094 — deep, merged master, substantive Guy Harris review/approval; !11109 and !11110 are release backports.** A BLF LIN trailer field is optional; treating it as a mandatory fixed-struct member caused valid files to fail. Guy explicitly notes that if the optional value is ever needed, presence can be determined from the message length before access. Strong corroboration of existing optional-field/parser-boundary guidance.
- **!11093 — merged master, approved/merged by John Thacker; !11103 and !11104 are release backports.** A missing byte in a fixed BLF CAN-FD record structure shifted subsequent fields and caused intermittent misdecoding. Reinforces validating binary-layout structures field-for-field against the format, especially when C structs are used as layout descriptions.
- **!11099 — merged master, John Thacker authored/merged; !11100 and !11101 are release backports.** Follow Stream avoids Qt's word-boundary search for uncontrolled long lines because it becomes O(N^2); fixed-width renderers that insert their own line breaks retain the nicer wrap mode. Useful UI-performance exemplar, but no separate notebook rule was needed in this run.
- **!11112 and !11111 — merged release backports, John Thacker.** XMPP moves cleanup into Wireshark's exception-cleanup mechanism so fuzz-triggered exceptions cannot bypass freeing the element tree. Weighted as corroboration because the master change is !10743, outside this batch and not yet reviewed.
- **!11081 — merged master, Gerald Combs authored/merged.** The GLib-to-C99 conversion tool handles directory, UTF-8 decode, and general open/read failures before rewriting a file. Useful tooling robustness evidence; no broader rule promoted because the exact Python exception style is implementation-specific.
- **!11085 — merged master, Martin Mathieson authored/merged.** Replaces full-width masks with `0x0`, corroborating the notebook's existing typed-field/mask conventions rather than introducing a new rule.
- **!11076 — merged master, John Thacker authored/merged.** Expands TCP conversation completeness into a bitmask with per-bit tree entries. Useful field-presentation example but no new cross-cutting convention was extracted.
- The remaining merged MRs were scanned for discussion/diff patterns and were either straightforward feature additions, release backports of a master change already represented in this batch, documentation/typo/image updates, narrow platform/UI fixes, or examples already covered by existing notebook guidance. They remain part of the exact reviewed set above even when they did not justify a durable notebook entry.

## Notebook changes committed in this run

- `reassembly-contiguous-frontier-conventions.md` — commit `9f9950e74c0918b369e99d03e8ca1fdc9051cbc3` — gap-closure correctness, first-gap optimization, and first-/second-pass reassembly testing from !11063 and !11102.
- `buffer-formatting-capacity-conventions.md` — commit `ba0f8fdecf0206eac2fb07d095896f79c9f2fbea` — upper-bound/NUL/capacity and semantic-emptiness rules from Guy Harris's !11098.
- `transformed-field-presentation-conventions.md` — commit `06c474b78b546598d6a59190086afbed379e20d8` — generated presentation for deprotected/derived values from !11079, corroborated by !11089.
- `heuristic-recognition-balance-conventions.md` — commit `7268205eb4b1177e3bc4800f5b6cfe9a9ebbc93f` — false-positive reduction versus malformed-packet debuggability from !11092.

## Frontier

The next descending candidate is **!11062, `wsdg backporting a patch: fix typo`**, which exists in the corpus at the same commit and is merged. It was checked only as the post-batch frontier and was **not** reviewed or counted in this run. The corpus therefore has not run out and no scraper restart is needed.
