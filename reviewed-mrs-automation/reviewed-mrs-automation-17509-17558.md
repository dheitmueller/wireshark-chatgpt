# Automated MR review: !17509–!17558

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest toward older MRs. Selection was made after consulting `reviewed-mrs.md` and the existing `reviewed-mrs-automation/` ledgers. The previously reviewed !17571–!17620 batch remains counted and was not re-reviewed.

## Exact MRs reviewed

!17558, !17557, !17556, !17555, !17554, !17553, !17552, !17551, !17550, !17549,
!17548, !17547, !17546, !17545, !17544, !17543, !17542, !17541, !17540, !17539,
!17538, !17537, !17536, !17535, !17534, !17533, !17532, !17531, !17530, !17529,
!17528, !17527, !17526, !17525, !17524, !17523, !17522, !17521, !17520, !17519,
!17518, !17517, !17516, !17515, !17514, !17513, !17512, !17511, !17510, !17509.

Count: **50**.

## Review notes

Merged MRs were weighted above abandoned, duplicate, superseded, or backport-only evidence. Maintainer discussion was weighted according to authority and specificity.

Notable evidence from this batch includes !17558, where modal Qt dialogs are given an owning main-window parent. Guy Harris noted that the behavior is relevant beyond Linux to X11/Wayland platforms generally and may also improve native dialog behavior on macOS. This is useful corroboration for ordinary Qt ownership/window-parent practice but does not justify a new notebook rule by itself.

!17550 corrects GSMTAP SIM presentation by avoiding radio-channel interpretation for the SIM type and instead displaying the SIM subtype. It is another example of keeping a field's presentation tied to the semantics of the active protocol/type rather than blindly applying an enum meaningful only in another context.

!17540 is a release-4.2 backport of `gen-bugnote` escaping work. Because it is a cherry-pick rather than the primary design discussion, it was treated as corroborating evidence only.

!17530 is a release-4.2 indentation-only WSLua cherry-pick authored/co-authored and merged by Guy Harris. It carries essentially no architectural evidence beyond consistency with project formatting.

!17520 is a release-4.4 data-table backport updating Art-Net OEM/ESTA assignments. As a backport with little review discussion, it was given low weight for durable conventions.

!17510 updates NTS-KE registry decoding to model IANA-assigned, unassigned, and reserved ranges explicitly with `range_string`, and uses the IANA registry as the source. This reinforces existing notebook guidance to model protocol registries according to their specified semantics and use Wireshark's standard value/range infrastructure rather than ad-hoc presentation.

## Notebook impact

No durable convention file was changed in this run. The useful findings corroborate existing guidance on semantic field presentation, standard value/range infrastructure, Qt object/window ownership, evidence weighting, and treating backports as corroboration rather than independent design evidence.

## Continuation

Rebuild the reviewed set from all tracking before the next run. Subject to that check, the next descending candidate after this batch is !17508.
