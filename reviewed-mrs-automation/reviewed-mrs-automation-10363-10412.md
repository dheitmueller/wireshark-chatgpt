# Wireshark MR automation ledger — !10412 through !10363

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed in this run, newest to oldest. Selection used individual MR membership from the accumulated review tracking rather than assuming a numeric range. `reviewed-mrs.md`, the aggregate automation ledger, the preceding exact ledger, and repository-wide exact-MR matches in per-run tracking were consulted. The preceding ledger listed !10412 only as an unreviewed frontier probe. The historical !17571–!17620 batch remains preserved and was revalidated as 50 reviewed MRs.

## Exact reviewed MR set

!10412, !10411, !10410, !10409, !10408, !10407, !10406, !10405, !10404, !10403, !10402, !10401, !10400, !10399, !10398, !10397, !10396, !10395, !10394, !10393, !10392, !10391, !10390, !10389, !10388, !10387, !10386, !10385, !10384, !10383, !10382, !10381, !10380, !10379, !10378, !10377, !10376, !10375, !10374, !10373, !10372, !10371, !10370, !10369, !10368, !10367, !10366, !10365, !10364, !10363

Count: **50 unique MRs**. Maximum: **!10412**. Minimum: **!10363**.

Merged: **47**. Non-merged/down-weighted: **3**:
- !10398 — closed and superseded by merged !10384.
- !10385 — open draft.
- !10379 — open.

## Review highlights

- !10412: project issue templates make representative captures first-class evidence for non-trivial bugs and enhancements.
- !10396, !10397, !10407: value-string fallback formatting is a correctness and crash-safety contract; `tools/check_val_to_str.py` checks it.
- !10405: new dissectors should have representative captures and fuzzing; keep packet access behind TVBuff-aware APIs.
- !10393 / !10395 / !10408: classify actual libpcap failures and attach only consequence-appropriate user guidance.
- !10389: do not route complete-PDU callers through a TCP/byte-stream framing entry point.
- !10376: the dissector `data` argument is part of the entry-point contract; different caller contexts merit separate wrappers over common code.
- !10371: shared media-type dispatch belongs in a neutral abstraction rather than an HTTP-owned one.
- !10367: named `register_dissector()` identities make reusable dissectors discoverable to `find_dissector()`, Lua, rawshark, and fuzzshark.
- !10374 and !10363: PMT-derived stream-type state drives generic MPEG-PES payload dispatch.

Full notes: `review-findings-10363-10412.md`.

## Next frontier

MR !10362 (`wmem: Allow integer lookups with a null tree`) exists at the same corpus commit, is merged to master, and is authored by John Thacker. It was inspected only to establish the next frontier and was not reviewed or counted.
