# Automated MR review ledger: !19109–!19158

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest unreviewed MR toward older MRs.

This run reviewed exactly 50 previously unreviewed merge requests. The reviewed set was selected after consulting the existing `reviewed-mrs-automation/` ledgers and the accumulated review history, including the preserved !17571–!17620 batch. No assumption was made that a numeric range was reviewed merely because a neighboring range appeared in a ledger.

## Exact reviewed set

!19158, !19157, !19156, !19155, !19154, !19153, !19152, !19151, !19150, !19149,
!19148, !19147, !19146, !19145, !19144, !19143, !19142, !19141, !19140, !19139,
!19138, !19137, !19136, !19135, !19134, !19133, !19132, !19131, !19130, !19129,
!19128, !19127, !19126, !19125, !19124, !19123, !19122, !19121, !19120, !19119,
!19118, !19117, !19116, !19115, !19114, !19113, !19112, !19111, !19110, !19109.

## Review notes

Merged MRs were weighted more heavily than abandoned, superseded, or transient backport/rework proposals. Reviewer discussion was weighted according to reviewer authority and specificity.

The strongest reusable evidence in this batch was corroborative of conventions already present in the notebook rather than establishing a genuinely new rule:

- !19119 adds a static checker for `tvb_get_bits<N>()` calls whose requested bit length exceeds the width of the selected accessor, and also notes cases where a narrower accessor would suffice. The same MR fixes two real 10-bit reads that incorrectly used `tvb_get_bits8()`, changing them to `tvb_get_bits16(..., ENC_BIG_ENDIAN)`. Martin Mathieson explicitly noted that the surrounding offsets indicate that the fields really occupy 10 bits. This reinforces the notebook's existing API-domain/type-width and checker-tooling guidance: choose an accessor whose semantic domain can represent the requested value, and encode mechanically detectable API-contract violations in project tooling where practical.
- !19129 fixes a display-filter crash caused by treating `BASE_UNIT_STRING` like an ordinary value-string representation, reinforcing the existing rule that APIs carrying tagged/variant semantics must be interpreted according to their actual discriminator rather than merely by pointer shape.
- !19124/!19142 add/fix OER uint64 and bit-string handling after missing type implementations caused crashes, reinforcing completeness requirements for shared decoding/type layers.
- !19110 returns normally from Qt `main()` instead of invoking application-exit machinery after the event loop is no longer running, reinforcing lifecycle ownership and teardown-boundary guidance.
- !19112 preserves the declared ordering of wiretap file extensions in the returned API list, reinforcing that ordering is part of an API contract when callers or UI presentation rely on it.
- !19121 fixes ownership of dynamically allocated stats-tree root-node memory, reinforcing existing allocation/lifetime conventions.
- !19128 contains several SMB2 dissector correctness improvements; !19115 improves TLS PSK binder tree structure; !19147 fixes BFCP attribute subtree lengths; these reinforce existing protocol-structure and tree-boundary guidance.
- The IEEE-11073 ftype sequence (!19131/!19135/!19136/!19138/!19145/!19146/!19149) is useful evidence for treating field types as semantic abstractions whose parsing, storage, display, unit formatting, and filtering behavior must remain internally consistent.

No separate convention file was changed because these findings duplicate or strongly corroborate conventions already represented in the notebook; adding another near-duplicate rule would reduce signal-to-noise.

Next descending candidate after this contiguous batch is !19108, subject to rebuilding the complete already-reviewed set and checking for any higher-numbered holes before the next run.
