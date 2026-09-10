# Wireshark MR review batch !24809–!24858

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base commit: `ed49944e3ec9d84b69074a734bae152843d8e713`

## Selection and prior-review accounting

Before selecting this batch, the reviewed-MR set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md` ledger, and all per-run files under `reviewed-mrs-automation/`. The union accounts for all MRs from !24859 through !26393 while preserving and counting the required historical !17571–!17620 batch. Selective tracking entries were treated individually; no numeric range was considered reviewed merely because some members appeared in a ledger.

After subtracting that exact reviewed set from the corpus and sorting the remaining MRs in descending MR-number order, the fifty highest-numbered previously unreviewed MRs are the batch below.

## Exact MRs reviewed

!24858, !24857, !24856, !24855, !24854, !24853, !24852, !24851, !24850, !24849,
!24848, !24847, !24846, !24845, !24844, !24843, !24842, !24841, !24840, !24839,
!24838, !24837, !24836, !24835, !24834, !24833, !24832, !24831, !24830, !24829,
!24828, !24827, !24826, !24825, !24824, !24823, !24822, !24821, !24820, !24819,
!24818, !24817, !24816, !24815, !24814, !24813, !24812, !24811, !24810, !24809.

Count: **50**.

## Review weighting and durable findings

Merged master MRs were treated as the strongest evidence; stable-branch backports corroborated accepted fixes. Open, abandoned, or superseded work was deliberately down-weighted. Reviewer rationale from established maintainers was given correspondingly high weight, including John Thacker's detailed fuzzing, portability, ownership, and parser feedback in this batch.

Durable notebook additions from this batch:

- **!24846, corroborated by !24850 and !24851 — use canonical alignment helpers instead of open-coded roundup arithmetic.** John Thacker's merged master fix replaces a local GDSDB 4-byte roundup expression that incorrectly advanced already-aligned values with `WS_ROUNDUP_4`; the same correction was carried to release-4.6 and release-4.4. Added to `arithmetic-safety-conventions.md`.
- **!24809 — constrain a wire value to its semantic range before applying a growth-producing transform.** John Thacker's OSS-Fuzz fix moves the RTPS domain-ID range check ahead of multiplication by `DOMAIN_GAIN`, preventing overflow in the intermediate port-offset calculation. Added to `arithmetic-safety-conventions.md`.
- **!24823 — prefer scope-managed ownership across exception-capable dissection paths.** John Thacker's OSS-Fuzz leak fix replaces GLib string splitting with `wmem_strsplit()` because a later TVB exception could bypass manual cleanup. Added to `allocator-scope-conventions.md`.

Strong corroboration that was not duplicated in the notebook includes !24856 and !24830 on semantic unsigned field domains; !24855, !24854, !24824, !24811, and !24810 on bounds, alignment, and parser robustness; !24836 on initializing observable state before early-error paths; !24847 on accounting for legal TCP corner cases when deriving completeness state; and !24814 on the Lua debugger pause/re-entry behavior already represented by later, stronger reentrancy evidence.

The !24819/!24825 pair strongly reinforces the existing tooling convention to encode deterministic structural problems in repository checkers: a bulk AI-assisted `#pragma once` conversion introduced UTF-8 BOMs, Gerald Combs identified the collateral file-format change during review and suggested a checker, and merged !24825 both removed the BOMs and added the check. This was treated as corroboration rather than a duplicate notebook rule.

Superseded/open work was down-weighted. **!24849** was closed because it had been submitted from the fork's `master` branch and was recreated as the merged **!24852**, so !24852 is the authoritative outcome. **!24821** remains open with a large evolving diff and a failed latest pipeline in the corpus snapshot, so its current architecture was not promoted as an accepted convention.

## Notebook files updated in this run

- `arithmetic-safety-conventions.md`
- `allocator-scope-conventions.md`
- `reviewed-mrs-automation/reviewed-mrs-automation-24809-24858.md`

`reviewed-mrs.md` was not modified; the exact per-run ledger above is authoritative for this batch.
