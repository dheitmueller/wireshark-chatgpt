# Wireshark MR review run: !13812 through !13763

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `1d0df05f28a9a20b898e48c455fef955ce5ce369`

Selection was reconciled against the available review tracking in `reviewed-mrs.md`, the aggregate automation tracking, and the per-run ledger inventory under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains part of the already-reviewed set. The preceding exact ledger covered !13862 through !13813 and explicitly treated !13812 only as a frontier probe, not as reviewed. The corpus remained at the same commit before and after this review, so exact set subtraction selected the fifty highest-numbered previously unreviewed corpus MRs listed below.

## Exact reviewed set

!13812, !13811, !13810, !13809, !13808, !13807, !13806, !13805, !13804, !13803,
!13802, !13801, !13800, !13799, !13798, !13797, !13796, !13795, !13794, !13793,
!13792, !13791, !13790, !13789, !13788, !13787, !13786, !13785, !13784, !13783,
!13782, !13781, !13780, !13779, !13778, !13777, !13776, !13775, !13774, !13773,
!13772, !13771, !13770, !13769, !13768, !13767, !13766, !13765, !13764, !13763.

Exactly 50 MRs were reviewed. Status mix: **49 merged; one open/draft (!13793)**. The open capture/GIOChannel refactor was treated as exploratory evidence and given substantially less architectural weight than accepted merged work.

## Durable findings promoted

- **!13809 — structured subprocess IPC must not share stderr with uncontrolled diagnostics.** John Thacker's merged master change stops forcing the `dumpcap` sync pipe onto file descriptor 2 because third-party/runtime diagnostics can legitimately write to stderr and corrupt the machine-readable framing. The accepted launch contract passes the sync-pipe descriptor or Windows handle explicitly. `subprocess-ipc-lifecycle-conventions.md` now records the dedicated-channel and explicit-inheritance rule.

- **!13777 — scope platform compatibility includes to the platforms that actually require them.** This merged master change was authored and merged by Guy Harris and therefore receives especially high weight. `net/if_var.h` was unnecessary and is not available on all BSDs; Guy explicitly says that if a future BSD build proves it is required, it should be conditionally included only for the BSD or BSDs that need it rather than restored for the whole family. `c-portability-conventions.md` now records this narrow OS-family portability rule.

- **!13771 — first-pass negotiated state should be conservative, not speculative.** John Thacker's merged TLS change handles ClientHello packets before an authoritative ServerHello/version-setting message is available. It displays the minimum client-supported version because that is a guaranteed lower bound while the final negotiated version is not yet knowable. The MR explicitly calls out live capture, one-pass TShark, and partial captures. `single-pass-analysis-conventions.md` now records the distinction between provisional guaranteed information and later authoritative state.

- **!13765 — already-established packet-level tap metadata should survive later dissection exceptions.** John Thacker's merged SCTP change initializes association identity/direction early and still queues association data when a later chunk read throws, provided enough of the packet was already successfully established. It also uses the protocol-defined first bundled chunk's association identity rather than opportunistically replacing packet-level state during later chunk parsing. `tap-data-contract-conventions.md` now records the exception-resilient publication rule.

- **!13797 — automatic selector registration must encode durable assigned-number namespace classes, not only today's occupied ranges.** Growth in the Bluetooth SIG member UUID registry exposed that ATT registration had excluded only currently known member subranges while still claiming other future `0xFxxx` values. The accepted master fix excludes the structurally reserved/member range so later registry updates do not silently create selector ownership conflicts. `dissector-entry-and-state-conventions.md` now records this future-registry-growth rule.

## Strong corroborating or review findings retained without duplicating notebook rules

- **!13812 and !13808** are stable/master forms of the IEC 61850 sampled-value validity semantics change; they reinforce keeping standards-derived labels and branch backports aligned.
- **!13804 and !13796** reinforce checker/source-of-truth discipline for generated dissectors: checker output should identify generated files, and fixes to generated ASN.1 dissectors belong in the generator/template source rather than only the emitted C.
- **!13803/!13791/!13790/!13789/!13788** reinforce reproducible registry maintenance and generated assigned-number refreshes rather than hand-editing large tables.
- **!13795** includes Jaap Keuter review steering 64-bit constants toward `G_GUINT64_CONSTANT`/`UINT64_C`, corroborating existing integer-width/arithmetic guidance.
- **!13786** refines SCTP association lookup so performance-oriented maps still require address-level disambiguation for incomplete half-associations; this corroborates the existing rule to key persistent state by the complete protocol relationship.
- **!13784 and !13767** use expert information for protocol elements formally deprecated by standards, reinforcing the distinction between successfully dissecting recognized wire values and diagnosing their standards status.
- **!13779** received direct Gerald Combs review of `g_strlcpy`: callers must honor the library's size contract rather than subtracting an extra byte when the API already reserves room for NUL termination. This corroborates existing C API-call contract guidance.
- **!13778** was prompted by Coverity CID 1544819 and fixes an E2AP registration range check, reinforcing treating static-analysis reports as possible real correctness/security bugs rather than merely silencing diagnostics.
- **!13776 with !13775** provides a clean CLI compatibility/deprecation example: the ambiguous no-argument `tshark -G` form is first documented and warned as deprecated, with the explicit `-G fields` replacement and release-note coverage, before eventual removal. The motivation also ties the awkward special parsing to POSIX guidance against optional arguments on short options.
- **!13773** stops LIN payload dissection when the link frame itself reports an error and makes the error visible in the Info column, reinforcing that higher-layer interpretation should not proceed when lower-layer status says the payload is invalid.
- **!13768** underwent extensive maintainer review before the PLDM Platform dissector was merged, including pre-commit/style cleanup. It is useful submission/process evidence but did not establish a stronger new convention than existing checker/style/test guidance.
- **!13763** is a release-4.0 Qt profile-menu lifetime/parenting backport; it reinforces QObject ownership/lifetime discipline but the accepted master behavior is stronger evidence than a stable backport.

The remaining MRs were stable backports, protocol-specific correctness fixes, generated registry updates, documentation/label corrections, and focused UI changes that did not establish a stronger durable project-wide rule. Merged work was weighted more heavily than the single open draft.

## Notebook commits from this run

- `4f929a35550ae2742426c1520973b1f9c147d6ed` — add dedicated subprocess IPC channel/explicit descriptor passing from !13809.
- `ba2b94f88b58d2790c0aa70483eddc8b139e9d30` — add Guy Harris's narrow platform-header portability rule from !13777.
- `9ba5c07c7aa12190ae3c637a05a1d77d879bce8a` — add conservative first-pass negotiated-state guidance from !13771.
- `e38c984f7b214d29047a4c12962497dd9b1a468f` — add exception-resilient packet-level tap publication rule from !13765.
- `7da9a500a90734730bee8213ee35bc9815d099e8` — add registry-growth/selector-ownership rule from !13797.

## Frontier

`mr_13762.json` exists in corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054` and is merged. It was fetched only to verify that the corpus continues and was **not** counted as reviewed. Therefore the corpus is not exhausted; absent newly scraped higher-numbered unreviewed material, !13762 is the next descending candidate.
