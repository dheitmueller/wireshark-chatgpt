# Automated Wireshark MR review: !16809-!16858

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest toward older MRs. Existing per-run ledgers and `reviewed-mrs.md` were consulted before selection. The historical !17571-!17620 batch remains part of the already-reviewed set. Selection was based on individual MR membership, not assumed numeric coverage.

## Exact reviewed set

!16858, !16857, !16856, !16855, !16854, !16853, !16852, !16851, !16850, !16849,
!16848, !16847, !16846, !16845, !16844, !16843, !16842, !16841, !16840, !16839,
!16838, !16837, !16836, !16835, !16834, !16833, !16832, !16831, !16830, !16829,
!16828, !16827, !16826, !16825, !16824, !16823, !16822, !16821, !16820, !16819,
!16818, !16817, !16816, !16815, !16814, !16813, !16812, !16811, !16810, !16809.

Count: 50.

## Review notes

Merged master changes were weighted most strongly; release backports, automatic updates, cosmetic cleanups, abandoned drafts, and superseded submissions were counted but given less independent architectural weight.

- **!16858 — DIS SISO enumeration expansion (closed, unmerged).** Large generated/enumeration-oriented proposal was closed without merge and had no substantive human review in the corpus snapshot; counted but given little architectural weight.
- **!16850 — Protobuf parsed-field storage (merged, master).** Replaces a fixed-array/overflow workaround with a `wmem_map_t` keyed representation for parsed fields. Martin Mathieson explicitly judged the map approach cleaner. Durable lesson: when protocol cardinality is naturally sparse/unbounded, prefer a data structure representing that domain rather than retaining a fixed-capacity array plus overflow workaround.
- **!16840 — GitLab CI interruptibility (merged, release-4.2 backport).** Moves cancellation semantics from globally interruptible jobs to merge-request-specific rules and `workflow:auto_cancel:on_new_commit: interruptible`. Useful CI behavior, but weighted below the originating master change because this is a cherry-pick.
- **!16830 — Qt <5.15 compatibility (merged, master).** Guards use of `QButtonGroup::idToggled`, introduced in Qt 5.15, because Wireshark still supported Qt 5.12. Reinforces existing minimum-supported-dependency guidance: do not infer API availability from a developer's local/current dependency version.
- **!16820 — wiretap compression dispatch cleanup (merged, master; John Thacker).** Replaces compile-time mazes around function signatures with a stable function shape and a runtime switch whose optional cases remain conditionally compiled. The explicit motivation is making additional compression types easier to add. Good architecture exemplar for isolating optional-feature conditionals at individual cases instead of letting them distort the surrounding API/control-flow structure.
- **!16810 — wiretap fast-seek reset cleanup (merged, master; Guy Harris).** Very high-authority exemplar. Uses an explicit switch over compression state, retains compile-time guards only inside relevant cases, provides extension points for future compression types, and asserts on impossible states when support/configuration invariants would otherwise be violated. This strongly corroborates the same optional-feature/state-machine organization seen in !16820.

## Durable conclusions

No separate convention file required modification in this run. The strongest findings reinforce guidance already represented in the notebook: model variable/sparse protocol state with an appropriate dynamic container instead of fixed-capacity overflow workarounds; code to the project's minimum supported dependency/API versions; keep optional-feature `#ifdef`s local to the feature-specific cases rather than allowing them to reshape APIs and function signatures; and make state machines explicit with exhaustive switches and assertions for genuinely impossible internal states.

## Continuation

Rebuild the reviewed set from all ledgers before the next run. If no higher-numbered hole is discovered, the next descending candidate is !16808.
