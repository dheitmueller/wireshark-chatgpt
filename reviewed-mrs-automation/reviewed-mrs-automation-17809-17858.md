# Automated MR review: !17809–!17858

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: newest toward older. Before selecting this batch, existing per-run ledgers in `reviewed-mrs-automation/` and `reviewed-mrs.md` were consulted. The historical !17571–!17620 batch remains part of the already-reviewed set. The immediately newer automation ledger ends at !17859, so after subtracting the full tracked reviewed set the fifty highest-numbered remaining corpus MRs are the exact set below.

## Exact reviewed MR set

!17858, !17857, !17856, !17855, !17854, !17853, !17852, !17851, !17850, !17849,
!17848, !17847, !17846, !17845, !17844, !17843, !17842, !17841, !17840, !17839,
!17838, !17837, !17836, !17835, !17834, !17833, !17832, !17831, !17830, !17829,
!17828, !17827, !17826, !17825, !17824, !17823, !17822, !17821, !17820, !17819,
!17818, !17817, !17816, !17815, !17814, !17813, !17812, !17811, !17810, !17809.

Count: **50**.

## Review notes

The batch was reviewed with merged MRs weighted above closed/abandoned work, and human discussion weighted according to reviewer authority and specificity.

- **!17809 (merged)** — `epan: Avoid dereferencing NULL pointer`. `FT_PROTOCOL` values for pseudo-protocols such as `_ws.expert` may legitimately have a NULL tvb. `value_get()` must preserve that semantic and return NULL rather than unconditionally constructing a subset tvb. This is a useful defensive API-boundary example, but it is already covered by the notebook's existing NULL/precondition guidance.
- **!17817 (closed)** — proposed GLib `GOptionGroup` integration for `wslog`. Gerald Combs questioned the concrete in-tree consumer because Wireshark executables did not use GOption; the author agreed it was primarily useful to external wsutil consumers or possible future unit-test utilities and ultimately closed it as not sufficiently worthwhile. This is useful negative design evidence: adding public/shared-library integration should have a concrete project use case and compatibility justification. Because the MR was closed, it is weighted as context rather than accepted architecture.
- **!17848 (merged, release-4.4)** — Falco/falcosecurity-libs version detection and compile-time compatibility handling. The accepted backport extracts the dependency version and uses explicit version checks around API differences rather than assuming one installed-library API. This corroborates existing dependency/version-boundary guidance.
- **!17858 (merged)** — one-character display-filter diagnostic typo correction. Correct but no durable engineering convention.

The remainder of the batch was scanned for merge outcome, purpose, diff shape, and substantive discussion. No additional convention was strong or novel enough to justify modifying the durable convention files in this run. In particular, accepted fixes in this range mostly reinforce already-recorded themes: validate optional/nullable representations at API boundaries, preserve compatibility across external dependency versions, use project tooling and shared helpers, and distinguish merged evidence from abandoned proposals.

## Notebook result

No durable convention file changed in this run. This ledger itself is the notebook update and records the exact reviewed set so later automation does not re-review these MRs.

## Continuation

For the next run, rebuild the already-reviewed set from all ledgers plus `reviewed-mrs.md` rather than assuming numeric ranges. With the corpus unchanged and absent any older independently reviewed entries that alter ordering, continue below !17809 while still preserving/counting !17571–!17620.
