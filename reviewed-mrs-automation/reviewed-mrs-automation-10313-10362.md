# Wireshark MR automation ledger — !10362 through !10313

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This is the authoritative exact-set ledger for this run. Exactly 50 previously unreviewed merge requests were reviewed. Selection was based on individual MR membership in the accumulated tracking and per-run ledger inventory, not inferred range coverage. The prior !10412–!10363 ledger mentioned !10362 only as a next-frontier probe, so that mention was not counted as a review. The historical !17571–!17620 batch remains preserved as 50 unique reviewed MRs.

## Exact reviewed MR set

!10362, !10361, !10360, !10359, !10358, !10357, !10356, !10355, !10354, !10353,
!10352, !10351, !10350, !10349, !10348, !10347, !10346, !10345, !10344, !10343,
!10342, !10341, !10340, !10339, !10338, !10337, !10336, !10335, !10334, !10333,
!10332, !10331, !10330, !10329, !10328, !10327, !10326, !10325, !10324, !10323,
!10322, !10321, !10320, !10319, !10318, !10317, !10316, !10315, !10314, !10313

Count: **50 unique MRs**. Maximum: **!10362**. Minimum: **!10313**.

Merged: **44**.

Closed/unmerged and down-weighted: **!10360, !10345, !10335, !10334, !10333, !10324**.

## High-value evidence

- !10343/!10352, !10341/!10351, and !10330/!10353: Guy Harris explicitly redirected generated-output fixes to the authoritative ASN.1/template sources; !10354–!10357 preserve the same source/output correction in stable branches.
- !10349: Guy Harris makes protocol ID -1 an explicit “no associated protocol” registration sentinel rather than relying on incidental negative-ID lookup behavior.
- !10345: John Thacker rejected a broad cross-dissector SRT timestamp abstraction in favor of redissection when time shift changes; the contributor agreed and closed the MR.
- !10332: recoverable invalid-argument contract diagnostics use INFO plus a dedicated InvalidArg log domain so fuzzing is useful by default while developers can selectively make the domain fatal.
- !10329: compiler-specific diagnostics test actual compiler identity rather than treating GNU-compatibility macros as proof of GCC.
- !10315: the typed-item checker substitutes simple mask macros before validating field/mask semantics.
- !10359 and !10347: genuine one-bit masked fields use FT_BOOLEAN with shared true/false strings; !10359 explicitly limits the mechanical conversion to non-zero-mask fields.
- !10362: integer wmem-tree lookup/contains operations tolerate a NULL tree consistently with string lookups, enabling lazy creation.
- !10361: H.264 Annex-B framing gets a distinct dissector entry point and bounds fixed-width start-code reads.

Full batch notes: `review-findings-10313-10362.md`.
