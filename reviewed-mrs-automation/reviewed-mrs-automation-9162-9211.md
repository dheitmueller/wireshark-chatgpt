# Wireshark MR automation ledger — !9211 through !9162

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This is the authoritative exact-set ledger for this run. Exactly 50 previously unreviewed merge requests were reviewed. Selection was made from individual MR membership in the accumulated review tracking and per-run ledgers, not by assuming numeric range coverage. Existing ledgers cover !9212 and above through the already-reviewed corpus frontier, while the historical !17571–!17620 batch was rechecked and remains exactly 50 unique reviewed MRs.

## Exact reviewed MR set

!9211, !9210, !9209, !9208, !9207, !9206, !9205, !9204, !9203, !9202, !9201, !9200, !9199, !9198, !9197, !9196, !9195, !9194, !9193, !9192, !9191, !9190, !9189, !9188, !9187, !9186, !9185, !9184, !9183, !9182, !9181, !9180, !9179, !9178, !9177, !9176, !9175, !9174, !9173, !9172, !9171, !9170, !9169, !9168, !9167, !9166, !9165, !9164, !9163, !9162

Count: **50 unique MRs**. Maximum: **!9211**. Minimum: **!9162**.

Merged: **45**.

Closed/unmerged and down-weighted: **!9192, !9189, !9180, !9171**.

Open/unmerged and down-weighted: **!9166**.

## High-value evidence

- !9183/!9184/!9185 and !9186: Guy Harris-authored path handling distinguishes URL paths, native filesystem path presentation, the current executable, and named executables in the main Wireshark program directory.
- !9168 with !9179/!9181: Gerald Combs plus direct Guy Harris review define `get_progfile_dir()` as the main Wireshark program directory even for extcaps; the bug fix was backported.
- !9169 with !9177/!9178: a parser must not add a fallback cursor increment if a subordinate length parser already advanced the offset.
- !9203: actionable preference-file syntax errors use the user-visible reporting path rather than console-only logging.
- !9201: John Thacker makes generic/custom-column rendering honor the same optional name-resolution semantics for IPv4, IPv6 and FCWWN fields.
- !9191: packet-driven SOME/IP port registration is restricted to endpoint options actually referenced by SOME/IP service entries.
- !9190: Coverity-driven corrections reinforce using one named field mask as the source of truth and widening an operand before multiplication rather than after overflow.
- !9182/!9187: Martin Mathieson's typed-item checker work demonstrates semantic checking of bit-oriented tree APIs and narrow treatment of known reserved/spare-field exceptions; later reviewed checker MRs provide the stronger final enabled-policy evidence.
- !9193 and !9172: representative captures materially improved review, including equal-timestamp edge cases, unknown enum values, portability diagnostics, and validation of changed/unchanged protocol paths.
- !9189: Martin Mathieson directly reiterates stable-branch policy: new work lands and stabilizes on master; stable release branches normally receive bug fixes rather than new features.

Full batch notes: `review-findings-9162-9211.md`.

## Next frontier

MR !9161 (`dfilter: Check if type supports unary minus`) exists at the same corpus commit and is merged. Its metadata was inspected only to establish the next descending frontier; it was not reviewed or counted in this run.
