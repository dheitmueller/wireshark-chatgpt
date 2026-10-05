# Wireshark MR automation ledger — !10362 through !10313

Corpus repository: `dheitmueller/wireshark-corpus-mrs`  
Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed merge requests, newest to oldest. Selection was made after reconciling the accumulated notebook tracking, including `reviewed-mrs.md`, the aggregate automation tracker, the per-run files under `reviewed-mrs-automation/`, and the immediately preceding exact ledger for !10412 through !10363. Candidate membership was checked by MR number rather than inferred from ledger filename ranges. The previous mention of !10362 was explicitly a frontier probe and did not count as a completed review.

The historical `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` batch was revalidated during this run: it contains exactly 50 unique MRs, minimum !17571 and maximum !17620, with no omissions.

## Exact reviewed MR set

!10362, !10361, !10360, !10359, !10358, !10357, !10356, !10355, !10354, !10353, !10352, !10351, !10350, !10349, !10348, !10347, !10346, !10345, !10344, !10343, !10342, !10341, !10340, !10339, !10338, !10337, !10336, !10335, !10334, !10333, !10332, !10331, !10330, !10329, !10328, !10327, !10326, !10325, !10324, !10323, !10322, !10321, !10320, !10319, !10318, !10317, !10316, !10315, !10314, !10313

Count: **50 unique MRs**. Maximum: **!10362**. Minimum: **!10313**.

Outcome weighting:
- **44 merged**
- **6 closed/unmerged**, down-weighted as implementation evidence: !10360, !10345, !10335, !10334, !10333, !10324.
- No open MRs in this batch.

## Review highlights

- !10330, !10341, and !10343 initially fixed generated dissector output. Guy Harris explicitly required the corresponding ASN.1 template/conformance sources to be fixed and the output regenerated. Merged follow-ups !10353, !10351, and !10352 do exactly that; Guy's stable-branch MRs !10354-!10357 preserve the same source/output parity.
- !10329 contains strong compiler-portability review from John Thacker: `__GNUC__` is a compatibility claim, not reliable proof of GCC. The accepted test excludes Clang, Intel Classic, and Intel LLVM before enabling GCC-only diagnostics. !10318 is the earlier version-gating precursor.
- !10331 documents that a file format can contain short magic values yet still need heuristic registration because those signatures are prone to false positives. !10328 documents the corresponding ordering rule: prefer more discriminating and faster heuristics ahead of weaker/slower ones.
- !10359 and !10347 convert one-bit semantic values from tiny integer value tables to `FT_BOOLEAN` fields with shared `true_false_string` definitions and carrier-appropriate widths.
- !10345 is closed/unmerged, but John Thacker's review is useful negative architecture evidence: when a global time-shift operation invalidates request/response timing, the simpler fix is to trigger redissection rather than introduce compensating timing APIs across many dissectors.
- !10315 improves `check_typed_item_calls.py` so masks expressed through macros can be checked, then fixes the real field-width/mask problems the expanded checker exposes.
- !10362 makes integer wmem-tree lookup/contains operations treat a NULL tree as an empty tree, matching string lookup behavior and enabling lazy allocation on first insertion.
- !10361 adds an H.264 Annex B bytestream dissector and guards the existing 32-bit start-code read for NAL units shorter than four bytes; its implementation explicitly notes that cross-packet NAL fragmentation would require stream reassembly support.
- !10349, authored by Guy Harris, preserves the documented sentinel contract that protocol ID -1 means “no protocol” when registering dissector tables, mapping it to NULL instead of passing -1 through ordinary protocol lookup.
- !10324 was closed after Stig Bjørlykke pointed out that the existing “Show Packet Bytes” UI already provides C and Rust array output; useful evidence to check adjacent UI functionality before adding a duplicate action.

Full per-MR notes: `review-findings-10313-10362.md`.  
Batch convention synthesis: `conventions-10313-10362.md`.

## Next frontier

MR !10312 (`DRDA: Support SQLATTR`) exists at the same corpus commit, is merged to `master`, and is authored by John Thacker. It was inspected only to establish the next frontier and was **not** reviewed or counted in this run.
