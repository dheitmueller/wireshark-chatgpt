# Run manifest: Wireshark MR review !3611–!3660

## Inputs

- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor branch: `automation/mr-review-3661-3710-authoritative`
- Notebook predecessor commit: `265872381b738122f3d1698731dca3e68b136d0a`
- Review model requirement: GPT-5.6 Sol or newer/more capable; satisfied.
- Direction: descending toward older MRs.

## Selection integrity

The batch was selected by exact MR membership rather than by assuming the numeric interval was wholly unreviewed. The predecessor exact ledger records !3710–!3661 and marks !3660 only as the next frontier probe. Root `reviewed-mrs.md`, the aggregate automation tracker, the predecessor reconciliation/manifest, and the preserved !17571–!17620 ledger were consulted before selection. No member of !3611–!3660 was found already reviewed.

The historical !17571–!17620 batch was revalidated as 50 unique reviewed MRs with no omissions.

## Reviewed batch

Exactly 50 MRs: !3660 descending through !3611 inclusive.

- Merged: 48
- Closed/unmerged: 2 — !3641 and !3623
- Closed work was used only as lower-weight supersession or branch-applicability history.
- No SMPTE 291/VANC packet type was encountered.

## Outputs

- `reviewed-mrs-automation/ledger-3611-3660.md`
- `reviewed-mrs-automation/review-findings-3611-3660.md`
- `reviewed-mrs-automation/conventions-3611-3660.md`
- `reviewed-mrs-automation/run-manifest-3611-3660.md`
- Durable findings are also promoted into topical convention files.

## Next frontier

!3610, `OSPF: Fixed SRLB and SRMS Preference TLV types (rfc8665)`, exists in the same corpus snapshot, is merged into release-3.4, and is **not** counted as reviewed. The corpus is not exhausted.
