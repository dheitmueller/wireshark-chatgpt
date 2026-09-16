# Automated MR review: !18609–!18658

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Reviewed on 2026-09-16. Selection was made after consulting `reviewed-mrs.md` and the accumulated files under `reviewed-mrs-automation/`, preserving and counting the historical !17571–!17620 batch. The established direction remains newest toward older. The previous run explicitly recorded !18659–!18708 as its exact set; this run records the next fifty corpus MRs not represented by the accumulated tracking.

## Exact reviewed set

!18658, !18657, !18656, !18655, !18654, !18653, !18652, !18651, !18650, !18649,
!18648, !18647, !18646, !18645, !18644, !18643, !18642, !18641, !18640, !18639,
!18638, !18637, !18636, !18635, !18634, !18633, !18632, !18631, !18630, !18629,
!18628, !18627, !18626, !18625, !18624, !18623, !18622, !18621, !18620, !18619,
!18618, !18617, !18616, !18615, !18614, !18613, !18612, !18611, !18610, !18609.

Count: **50**.

## Review notes

Merged MRs were weighted more strongly than closed/superseded work. Routine release/CI/registry changes and small protocol corrections were scanned without promoting them merely to create notebook churn.

- **!18658 — Merged.** Corrects the wkssvc NetrWkstaGetInfo server-name NDR pointer type to UNIQUE and fixes dependent IDL include paths. It is the successful replacement for closed !18657, whose narrower change identified the same REF-vs-UNIQUE source-data problem. The merged successor is the authoritative exemplar.
- **!18655 — Merged.** Adds iSCSI login/text key-value dissection and display filters. Useful protocol visibility improvement, but no new general convention was extracted.
- **!18646 — Merged, John Thacker.** Keeps the actual sFlow sampled-header length distinct from the XDR-aligned amount consumed from the enclosing stream: the unpadded length is used for the field/subdissection, while `WS_ROUNDUP_4(header_length)` is used only when advancing the outer offset. This is strong corroboration of the existing notebook rule that source/framing bytes consumed and semantic payload length must remain distinct when padding/alignment exists.
- **!18632 — Merged.** sFlow Ethertype display uses the existing Ethertype value-string table rather than inventing a local mapping; useful helper/table reuse corroboration.
- **!18631 / !18613 — Merged O-RAN FH CUS corrections.** Focused subtree/protocol-validity fixes with little reusable human-review evidence beyond accepted implementation.
- **!18625 — Merged.** NTP removes duplicated extension parsing by allowing guarded recursion. This is a good local simplification but does not justify a general recursion rule beyond existing parser-state guidance.
- **!18622 / !18621 / !18619 / !18618 / !18617 / !18615 / !18614 — Merged Guy Harris Buffer/wiretap cleanup sequence.** The sequence makes constness explicit, replaces macro-like Buffer operations with inline routines, centralizes reset semantics, and moves `ws_buffer_clean(&rec->data)` into `wtap_init_rec()` so the operation documented as resetting a `wtap_rec` actually resets its owned data buffer too. !18614 explicitly documents `wtap_init_rec()` as returning the complete record to an initialized state and notes that other buffers should be audited. This is high-authority corroboration of the existing lifecycle/ownership convention: reset/initialize operations should restore the whole owning abstraction rather than requiring callers to know and separately reset internal members.
- **!18657 — Closed/superseded.** Earlier wkssvc pointer-type fix; weighted below merged !18658.

No convention file was modified in this run. The strongest durable findings independently corroborate existing notebook guidance on ownership/reset completeness and keeping semantic payload lengths distinct from padded/framing consumption; adding duplicate wording would reduce the notebook's signal-to-noise ratio.

## Continuation

Continue backward from the highest-numbered corpus MR not present in the rebuilt reviewed set. Do not assume !18608 is next without consulting all tracking for holes or previously reviewed out-of-order MRs.
