# Wireshark MR automation ledger — !10362 through !10313

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This is the authoritative exact-set ledger for this run. Exactly 50 previously unreviewed merge requests were reviewed after consulting the available accumulated tracking in `reviewed-mrs.md`, the per-run ledgers under `reviewed-mrs-automation/`, and the immediately preceding branch-only !10412–!10363 ledger. Candidate membership was checked by MR number rather than assuming numeric range coverage. The historical !17571–!17620 batch remains preserved and counted.

| MR | Outcome |
|---|---|
| !10362 | merged |
| !10361 | merged |
| !10360 | closed draft / down-weighted |
| !10359 | merged |
| !10358 | merged |
| !10357 | merged |
| !10356 | merged |
| !10355 | merged |
| !10354 | merged |
| !10353 | merged |
| !10352 | merged |
| !10351 | merged |
| !10350 | merged |
| !10349 | merged |
| !10348 | merged |
| !10347 | merged |
| !10346 | merged |
| !10345 | closed / superseded by redissection approach / down-weighted |
| !10344 | merged |
| !10343 | merged |
| !10342 | merged |
| !10341 | merged |
| !10340 | merged |
| !10339 | merged |
| !10338 | merged |
| !10337 | merged |
| !10336 | merged |
| !10335 | closed / automatic update missed window / down-weighted |
| !10334 | closed / automatic update missed window / down-weighted |
| !10333 | closed / automatic update missed window / down-weighted |
| !10332 | merged |
| !10331 | merged |
| !10330 | merged |
| !10329 | merged |
| !10328 | merged |
| !10327 | merged |
| !10326 | merged |
| !10325 | merged |
| !10324 | closed / existing functionality already covered use case / down-weighted |
| !10323 | merged |
| !10322 | merged |
| !10321 | merged |
| !10320 | merged |
| !10319 | merged |
| !10318 | merged |
| !10317 | merged |
| !10316 | merged |
| !10315 | merged |
| !10314 | merged |
| !10313 | merged |

## Exact reviewed MR set

!10362, !10361, !10360, !10359, !10358, !10357, !10356, !10355, !10354, !10353, !10352, !10351, !10350, !10349, !10348, !10347, !10346, !10345, !10344, !10343, !10342, !10341, !10340, !10339, !10338, !10337, !10336, !10335, !10334, !10333, !10332, !10331, !10330, !10329, !10328, !10327, !10326, !10325, !10324, !10323, !10322, !10321, !10320, !10319, !10318, !10317, !10316, !10315, !10314, !10313

Count: **50 unique MRs**. Maximum: **!10362**. Minimum: **!10313**.

Merged: **44**. Closed/unmerged: **6** (!10360, !10345, !10335, !10334, !10333, !10324).

## Promoted and corroborating findings

- !10329 and !10318: compiler-specific diagnostics must use real compiler-family/version detection; `__GNUC__` compatibility alone does not establish GCC identity.
- !10330, !10341 and !10343, with Guy Harris corrective MRs !10353, !10351 and !10352: generated ASN.1 dissector output is not the source of truth; fix the template/conformance input and regenerate.
- !10315: repository source checkers should resolve simple mask macros before applying width/contiguity rules; the stronger analysis immediately exposed real hf type/mask defects.
- !10332: recoverable argument-contract failures should remain diagnosable without default fatal severity that turns them into fuzzer-stopping events; a dedicated log domain allows opt-in fatal debugging.
- !10345 was closed after John Thacker identified whole-capture redissection on time shift as the simpler correct architecture, strongly corroborating the already-recorded redissection rule.
- !10324 was closed when review identified existing “Show Packet Bytes → C/Rust Array” functionality that already satisfied the use case; useful negative evidence for checking existing capability before expanding UI surface.
- !10362 makes integer-key wmem lookup behavior consistent with string lookup for a lazily absent tree by returning NULL/FALSE rather than dereferencing NULL.
- !10349 makes the `proto == -1` dissector-table registration sentinel explicitly mean “no associated protocol”, preserving an intentional API contract.
- !10361, !10342 and !10322 are merged John Thacker MPEG/H.264 robustness work; they were reviewed but did not justify broader rules beyond existing framing/bounds guidance.

Full batch notes: `review-findings-10313-10362.md`.

## Next frontier

MR !10312 (`DRDA: Support SQLATTR`) exists in the same corpus commit, is merged, and was checked only as the next-frontier probe. It was not counted in this run.
