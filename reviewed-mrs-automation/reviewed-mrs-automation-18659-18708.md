# Automated MR review: !18659–!18708

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Reviewed on 2026-09-16. Selection was made by rebuilding the already-reviewed set from `reviewed-mrs.md` and all accumulated files under `reviewed-mrs-automation/`, preserving and counting the historical !17571–!17620 batch. These are the fifty highest-numbered corpus MRs not already represented in that tracking at selection time. Review direction remains newest toward older.

## Exact reviewed set

!18708, !18707, !18706, !18705, !18704, !18703, !18702, !18701, !18700, !18699,
!18698, !18697, !18696, !18695, !18694, !18693, !18692, !18691, !18690, !18689,
!18688, !18687, !18686, !18685, !18684, !18683, !18682, !18681, !18680, !18679,
!18678, !18677, !18676, !18675, !18674, !18673, !18672, !18671, !18670, !18669,
!18668, !18667, !18666, !18665, !18664, !18663, !18662, !18661, !18660, !18659.

Count: **50**.

## Review notes

Merged MRs were weighted more strongly than drafts, abandoned work, or superseded proposals. Automatic updates, release preparation, simple backports, spelling/registry changes, and similarly low-information changes were scanned but not promoted merely to create notebook churn.

- **!18708 — Merged release backport.** QUIC/sharkd test is skipped when Brotli support is absent because the expected result depends on decompression capability. This is the release-4.4 counterpart of the already reviewed master-side capability-sensitive test fix and adds no new convention.
- **!18707 — Merged, John Thacker.** `WS_LOG_DOMAIN` must be defined before any header transitively includes `wsutil/wslog.h`; otherwise the intended log domain has no effect. This is a useful C/preprocessor ordering detail but is sufficiently specific that it does not warrant a separate general notebook rule.
- **!18690 — Merged.** HI2Operations ASN.1 configuration fixes APN display and deduplicates fields. Human review caught a syntax-level extra comma and the correction was made before merge; useful ordinary review hygiene but no new durable architecture rule.
- **!18680 — Merged, John Thacker.** Standardizes libpcap includes on `pcap/pcap.h` (and `pcap/bpf.h`) based on the project's minimum supported libpcap version. This is a good dependency-baseline exemplar: once the minimum supported dependency guarantees a canonical interface, compatibility probing for obsolete layouts can be removed rather than indefinitely retained.
- **!18678 — Merged, Martin Mathieson.** O-RAN FH CUS SE2 fixes; focused protocol correction with successful pipeline and no substantive human design discussion beyond approval. No additional convention extracted.
- **!18675 — Deep, merged, Guy Harris authored.** Refactors the wiretap dump path so the record and its payload buffer are one coherent object rather than parallel arguments. John Thacker caught a Windows-only `extcap/etl.c` caller that the initial cross-platform build coverage missed. The architectural direction is strong: avoid APIs carrying redundant parallel representations of state when one owning object already contains the other; the review also reinforces auditing platform-specific callers when changing widely used interfaces. Both lessons are already represented by broader notebook guidance on ownership/state coherence and cross-platform call-site auditing, so no duplicate rule was added.
- **!18670 / !18696 / !18659 — Merged release/automatic maintenance.** Release preparation, release build, and automatic registry/documentation updates. Low reusable engineering signal for current notebook purposes.

No durable convention file was modified in this run because the strongest findings corroborate existing notebook guidance or are narrow implementation details. Avoiding redundant or over-specific rules is preferable to adding notebook churn for every batch.

## Continuation

Continue backward from the highest-numbered corpus MR not present in the rebuilt reviewed set; do not assume !18658 is necessarily next without checking all tracking for holes or previously reviewed out-of-order MRs.
