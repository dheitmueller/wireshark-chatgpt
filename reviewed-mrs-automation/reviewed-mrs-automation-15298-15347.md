# Wireshark MR review automation — 2026-09-20

Model: GPT-5.6 Sol

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

## Selection and tracking

Before selecting this batch, the already-reviewed set was reconstructed from all available review tracking in `dheitmueller/wireshark-chatgpt`: `reviewed-mrs.md`, the aggregate automation tracking, and the per-run files under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains explicitly counted. Candidate MR numbers were checked against the exact reviewed set rather than inferring that numeric ranges were complete.

The immediately preceding run reviewed through !15348 (with the corpus's empty `mr_15376.json` correctly excluded from that earlier 50-MR count). The fifty highest-numbered valid corpus MRs not already in the reviewed set are therefore !15347 through !15298 inclusive. All 50 records were individually inspected, including their available discussions and diffs.

## Exact MRs reviewed in this run

!15347, !15346, !15345, !15344, !15343, !15342, !15341, !15340, !15339, !15338

!15337, !15336, !15335, !15334, !15333, !15332, !15331, !15330, !15329, !15328

!15327, !15326, !15325, !15324, !15323, !15322, !15321, !15320, !15319, !15318

!15317, !15316, !15315, !15314, !15313, !15312, !15311, !15310, !15309, !15308

!15307, !15306, !15305, !15304, !15303, !15302, !15301, !15300, !15299, !15298

Count: **50 MRs**.

Status weighting: **46 merged**, **4 closed/unmerged** (!15346, !15327, !15318, and !15298). The closed MRs were down-weighted. !15346 was superseded by accepted !15345; !15298 was superseded by accepted !15299. !15318 was an unsuccessful release-3.6 form of DSB lifetime work whose accepted master and other supported-branch variants carry greater weight. Stable-branch backports were treated mainly as corroboration rather than independent architectural evidence.

## Durable findings promoted to the notebook

### Preserve unknown/not-supplied separately from a known false metadata value

Merged master !15344 extends Peekremote V0 capture metadata with 6 GHz information. During review, Alexis La Goutte asked why the design needed both `6GHZ_BAND_VALID` and `IS_6GHZ`. The contributor explained that older sniffer devices do not know about the new field; without a validity bit, their zero value would be misread as a known 2.4/5 GHz result instead of unknown. The accepted representation retains the presence/validity dimension. Added as `capture-metadata-presence-conventions.md`.

Notebook commit: `239b0c6442e9cbd18af2360023578bfe2bf5d329`

### Budget fixed serialized-option overhead before accepting variable payload length

Merged master !15322, authored and merged by Guy Harris, fixes pcapng `IDB_FILTER` length accounting. The option body has a 16-bit maximum but includes a one-byte filter-type discriminator, so the filter expression must fit below `UINT16_MAX - 1`; the preflight/counting path had used the looser `UINT16_MAX` check even though the actual writer used the correct bound. Stable !15323, !15324, and !15325 corroborate the fix. This was given especially high authority because it is direct Guy Harris implementation evidence. Added as `capture-option-length-budget-conventions.md`.

Notebook commit: `9dddbc49b9fc64be0aba5be6f9b69421f2708a12`

### Column delimiters must compose correctly for both singleton and repeated items

Merged master !15334 adds callable NR-RRC SIB dissectors and adjusts Info-column formatting. Pascal Quantin explicitly rejected an intermediate rendering because it would not display correctly when multiple SIBs appeared in the same System Information message. The accepted implementation handles the single-SIB and multi-SIB forms coherently. Added as `packet-column-composition-conventions.md`.

Notebook commit: `42b25f2e5106e69e4533024d0871685cddb9f92f`

### Reassembly completion must not depend on capture arrival order

Merged master !15300 fixes ISO 15765 out-of-order reassembly. An early-arriving logical final Consecutive Frame could previously have all of its bytes, including padding, accepted as payload, causing later real fragments to be truncated. The accepted code derives actual fragment contribution from ISO 15765-2 geometry and tracks logical completion independently of arrival order. Added as `reassembly-arrival-order-conventions.md`.

Notebook commit: `9d4677fb0259f2d67035fb56a8b79332c1f7ce77`

## Additional evidence retained without duplicate notebook rules

- !15347 is a stable branch version of Guy Harris's TIPC byte-count correction; it reinforces using the same semantic length value for both data accounting and presentation rather than an unrelated padding variable.
- !15343/!15342/!15341/!15338 are Guy Harris TIPC alignment fixes/backports. They reinforce the already-recorded rule that an already four-byte-aligned value has zero padding, not four bytes, so no duplicate convention was created.
- !15336 (Anders Broman) makes PER's internal/display helper respect whether the target `hf_` field is signed or unsigned. This corroborates the existing protocol-field semantic rule that extraction domain, field signedness, and display formatting must agree.
- !15335 fixes CQL prepared-result handling, including negative sentinel lengths for NULL/unset values and avoiding metadata-dependent crashes; useful parser evidence, but covered by existing semantic-domain and malformed-input guidance.
- !15320 and !15319 are accepted stable-branch versions of the DSB reference-counted aggregate lifetime work already promoted from master !15660: retaining the outer array is insufficient unless its contained blocks are also ref-counted appropriately.
- !15316 contains useful parser/lifetime hygiene around rejecting values outside the intended unsigned domain and clearing list heads after deletion; existing numeric-domain and lifetime conventions already cover the durable lesson.
- !15302 replaces legacy Win32 file/export dialogs with the common Qt implementations, primarily to reduce platform-specific maintenance and DPI behavior differences. This is useful UI architecture evidence but not strong enough in this batch to justify a new universal rule.
- !15299 is the accepted MACsec decryption implementation replacing closed !15298. Its validation includes no-key behavior, wrong-key/authentication failure, correct-key decryption, and deliberately invalid packets; this is strong corroborating evidence for testing security-sensitive dissector changes across success and failure paths.
- !15346, !15327, !15318, and !15298 were closed without merge and were not treated as durable implementation authority where accepted replacements or later fixes existed.

## Continuation

The corpus is not exhausted. `mr_15297.json` exists and contains a valid merged MR record at the same corpus commit, so the next review run can continue backward from !15297. No scraper-restart notification is needed for this run.