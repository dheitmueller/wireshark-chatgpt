# Wireshark MR automation review: !25659-!25708

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to older. This run reviewed exactly 50 previously unreviewed merge requests: !25708 down through !25659. The existing `reviewed-mrs.md`, the per-run files in `reviewed-mrs-automation/`, and the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md` were consulted before finalizing the batch. The already-reviewed !17571-!17620 batch remains preserved and was not repeated.

`Deep` means the implementation/diff and relevant discussion were examined for reusable engineering evidence. `Discussion-focused` means reviewer discussion was the primary durable evidence. `Scanned` means metadata, purpose/outcome, discussion significance, and implementation context were checked but no additional durable convention justified deeper promotion.

| MR | Review | Notes |
|---|---|---|
| !25708 | Scanned | Merged Guy Harris dumpcap comment cleanup; platform-specific comments moved under the same Windows/non-Windows preprocessor branches as the code they describe. No new rule beyond keeping comments aligned with active platform paths. |
| !25707 | Scanned | Merged John Thacker Qt Export Objects fix initializing a member variable. Narrow correctness follow-up; no additional durable convention. |
| !25706 | Deep | Merged John Thacker HTTP/3 Export Objects and request/response tracking. Adds transaction frame links/timing/URI propagation and documents lifetime requirements for strings handed to the generic HTTP export-object helper. Useful accepted HTTP/3 state/lifetime exemplar, but existing notebook lifetime/state rules already cover the broader principle. |
| !25705 | Discussion-focused | Merged HiSLIP 2.0 expansion with TLS/plaintext switching and authentication support. Jaap Keuter requested squashing the development commit series; contributors did so before merge. Corroborates the existing clean/focused-commit submission rule rather than creating a duplicate convention. |
| !25704 | Discussion-focused | Merged PROFINET update to a newer specification. Jaap Keuter explicitly challenged historical-capture compatibility before removal/renaming of old wire definitions; contributor verified the affected forms had never shipped in released stacks. Promoted the compatibility-review lesson to `review-patterns.md`. |
| !25703 | Scanned | Merged PROFINET v2.5 CIM/syslog structured-data work. Protocol-specific feature/fix with no additional broadly reusable review convention selected from this batch. |
| !25702 | Scanned | Merged dissector script-warning fixes. Mechanical/static-check cleanup; no distinct durable lesson beyond existing pre-submit tooling guidance. |
| !25701 | Scanned | Merged dumpcap architecture-comment clarification. Useful source documentation but no additional general rule beyond keeping comments synchronized with real capture-process architecture. |
| !25700 | Scanned | Merged dumpcap removal of code disabled since 2005. Historical dead-code cleanup; no new durable convention. |
| !25699 | Discussion-focused | Merged editcap `--discard-name-resolution`. Guy Harris clarified that the underlying NRB discard semantics appropriately cover hostname mappings to any address type, not only IP. Useful semantic precision, but too feature-specific to promote as a standalone rule. |
| !25698 | Scanned | Merged hosts-file load-order update adding the base personal hosts file. Configuration-order behavior; no new notebook convention. |
| !25697 | Scanned | Merged stable-branch UMTS-FP bounds backport. Same capacity/limit hardening as the master fix; no independent lesson. |
| !25696 | Scanned | Merged stable-branch Z39.50 dynamic-directory-array backport. Corroborates !25691 without additional review evidence. |
| !25695 | Scanned | Merged stable-branch Z39.50 dynamic-directory-array backport. Corroborates !25691 without additional review evidence. |
| !25694 | Scanned | Merged c-ares callback const-correctness compatibility fix. Dependency API adaptation; no broader rule beyond ordinary upstream API compatibility. |
| !25693 | Scanned | Merged stable-branch UMTS-FP bounds backport. No additional lesson beyond the master capacity fix. |
| !25692 | Deep | Merged UMTS-FP channel/MAC-PDU bounds hardening for malformed input. Strong independent capacity-validation evidence, but the notebook already contains the fixed-capacity guard rule from later reviewed MRs. |
| !25691 | Deep | Merged John Thacker Z39.50 overflow fix. A floor-divided precomputed array count was not an upper bound because a partial record could still be appended; accepted code uses `wmem_array_t` and derives count from entries actually parsed. Promoted to `container-api-conventions.md`. |
| !25690 | Scanned | Merged O-RAN FH CUS ST10 measurement-type decoding. Protocol-specific extension; no new durable convention. |
| !25689 | Deep | Merged HTTP/3 API/state enhancement exposing the relevant stream ID for DoH use. Useful layered-protocol context API example; broader explicit-context principles are already represented in `architecture.md`. |
| !25688 | Scanned | Merged stable-branch IEEE 802.11 EAPOL bounds backport. No new evidence beyond the master validation fix. |
| !25687 | Scanned | Merged stable-branch IEEE 802.11 EAPOL bounds backport. No new evidence beyond the master validation fix. |
| !25686 | Scanned | Merged TCP RST diagnostic-payload draft update. Specification-tracking change; no additional general convention. |
| !25685 | Scanned | Merged O-RAN FH CUS linked-U-plane presentation enhancement. Protocol/UI-specific state display; no new convention. |
| !25684 | Scanned | Merged Guy Harris dumpcap comment correction. Documentation-only maintenance; no separate rule. |
| !25683 | Deep | Merged Livewire use-after-free fix for duplicate tree keys. Replacement can alias the value already stored under the key, so blindly freeing the old value can free the object being retained. Martin Mathieson also requested comments around the non-obvious mutation. Useful ownership evidence; existing canonical ownership/lifetime guidance is sufficient. |
| !25682 | Deep | Merged IEEE 802.11 EAPOL hardening rejects oversized/bogus parsed values before passing them into cryptographic inspection paths. Corroborates existing validate-before-dependent-processing guidance. |
| !25681 | Scanned | Merged spelling fixes. No engineering convention. |
| !25680 | Deep | Merged John Thacker macOS runtime-availability fix for `strchrnul`: a newer SDK can expose a function absent from the older supported deployment runtime. Uses a runtime availability check and fallback. Promoted to `build-conventions.md`. |
| !25679 | Deep | Merged Guy Harris error-path close API for compressed output. After an already-reported write/flush failure, the new path skips semantic finalization, releases buffers, and closes the descriptor. Promoted to `memory-lifetime-conventions.md`. |
| !25678 | Deep | Merged John Thacker HTTP/3 body reassembly/decompression and media-type dispatch. Refuses content-type subdissection when decompression is unavailable/failed or representation is partial, and models stream completion using Content-Length/QUIC FIN semantics. Strong accepted implementation, mostly corroborating existing validation/reassembly rules. |
| !25677 | Deep | Merged `ws_strchrnul` configure/fallback introduction. Follow-up !25680 demonstrated why compile-time capability alone was insufficient on macOS deployment targets; considered together with !25680 in the build rule. |
| !25676 | Deep | Merged QUIC state fix preserving a frame's previously detected gap status when later PDUs in the same frame are processed. Useful monotonic aggregation-state example; not generalized from one narrow path. |
| !25675 | Scanned | Merged O-RAN FH CUS result-entry frame-list initialization fix. Narrow state-shape correction; no separate convention. |
| !25674 | Scanned | Merged Guy Harris dumpcap output-initialization cleanup simplifying success/error flow. High-authority cleanup but no distinct rule beyond existing error-path structure guidance. |
| !25673 | Scanned | Merged Windows Qt AltGr detection configuration. Platform/UI-specific fix; no broad convention. |
| !25672 | Scanned | Merged automatic data/translation update. No reusable engineering lesson. |
| !25671 | Scanned | Merged automatic data/translation update. No reusable engineering lesson. |
| !25670 | Scanned | Merged automatic data/translation update. No reusable engineering lesson. |
| !25669 | Scanned | Merged Guy Harris dumpcap comment cleanup. No separate convention. |
| !25668 | Deep | Merged Guy Harris dumpcap file-finishing refactor separating final Interface Statistics Block emission from lower-level close mechanics. Together with !25679, supports distinct successful-finalization versus failure-abort lifecycles. |
| !25667 | Deep | Merged John Thacker reassembly API addition for ordered logical fragments where the known completion contract is total octets rather than fragment count. Factors common implementation while exposing semantics appropriate to the caller; useful accepted reassembly-API exemplar. |
| !25666 | Scanned | Merged stable-branch TLS ECH duplicate-extension hardening backport. No additional evidence beyond !25665. |
| !25665 | Deep | Merged John Thacker TLS ECH hardening enforces the protocol's single-`ech_outer_extensions` cardinality before copying extension data, preventing decompression/amplification-driven buffer growth. Strong security invariant exemplar; existing parser-boundary guidance already covers the broader rule. |
| !25664 | Scanned | Merged stable-branch backport making static error buffers thread-local. Corroborates !25662. |
| !25663 | Scanned | Merged stable-branch backport making static error buffers thread-local. Corroborates !25662. |
| !25662 | Deep | Merged Guy Harris change makes mutable static buffers in `wtap_strerror()` and file error-message helpers `WS_THREAD_LOCAL`, preserving reusable-buffer API lifetime without cross-thread races/overwrites. Promoted to `memory-lifetime-conventions.md`. |
| !25661 | Scanned | Merged stable-branch sdjournal whitespace cleanup. No engineering lesson. |
| !25660 | Deep | Merged Gerald Combs `fvalue_new_pool()` API allocates dissection-lifetime fvalues directly from the supplied wmem pool instead of separate GLib slice allocation/free. Strong allocator-scope exemplar, but this convention is already well represented in the notebook. |
| !25659 | Scanned | Merged Guy Harris sdjournal whitespace cleanup. Style-only; no additional durable convention. |

## Durable notebook updates from this run

- `container-api-conventions.md`: growable containers when parser-produced cardinality cannot be safely predicted from input byte counts (!25691).
- `build-conventions.md`: distinguish SDK/compile-time function availability from deployment-runtime availability (!25677, !25680).
- `memory-lifetime-conventions.md`: thread-local reusable static formatting buffers (!25662, with !25663/!25664 backports) and emergency error-close versus successful finalization (!25679, supported by !25668).
- `review-patterns.md`: explicitly assess historical-capture compatibility when a newer protocol specification removes or changes previously decoded wire definitions (!25704); !25705 also increases confidence in the existing clean/squashed commit-history guidance.
