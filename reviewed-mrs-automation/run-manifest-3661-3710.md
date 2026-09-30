# Run manifest: Wireshark MR review !3661–!3710

## Inputs

- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor branch: `automation/mr-review-3711-3760-authoritative`
- Notebook predecessor commit: `78299107c1da737dcfc8422bb0cf3d96fe2a571c`
- Review model requirement: GPT-5.6 Sol or newer/more capable; satisfied.
- Direction: descending toward older MRs.

## Selection integrity

The batch was selected by exact MR membership rather than by assuming a numeric interval was wholly unreviewed. Review tracking on the predecessor tree was inventoried; root `reviewed-mrs.md`, the aggregate automation tracker, the immediately preceding exact ledger, and all 20 irregular gap/backfill/noncontiguous/exact-list/reconciliation trackers were checked for candidate membership. None recorded !3661–!3710 as reviewed.

The preserved historical batch !17571–!17620 was revalidated as 50 unique reviewed MRs with no omissions.

## Reviewed batch

Exactly 50 MRs: !3710 descending through !3661 inclusive.

- Merged: 48
- Closed/unmerged: 2 — !3695 and !3677
- Closed work was used only as lower-weight supersession history.
- No SMPTE 291/VANC packet type was encountered.

## Outputs

- `reviewed-mrs-automation/ledger-3661-3710.md`
- `reviewed-mrs-automation/review-findings-3661-3710.md`
- `reviewed-mrs-automation/conventions-3661-3710.md`
- `reviewed-mrs-automation/run-manifest-3661-3710.md`
- Selected durable findings are also promoted into appropriate topical convention files on this branch.

## Next frontier

!3660, `Rename LONGOPT_NUM_CAP_COMMENT to LONGOPT_CAPTURE_COMMENT.`, exists in the same corpus snapshot, is merged on master, and was authored by Guy Harris. It was inspected only to establish the next frontier and is **not** counted as reviewed in this run. The corpus is therefore not exhausted.
