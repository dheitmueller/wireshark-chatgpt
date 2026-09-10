# Wireshark MR Review Automation Ledger — !24559–!24608

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed MR set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all per-run files under `reviewed-mrs-automation/`, counting individual MR entries rather than assuming that any numeric range was completely reviewed. The historical !17571–!17620 batch remains explicitly preserved and counted. Existing tracking also confirms the previously discontinuous !25759–!25827 area is already covered. The fifty highest-numbered corpus MRs not in the reconstructed set were !24608 through !24559 inclusive.

| MR | Review depth | Outcome / durable evidence |
|---|---|---|
| !24608 | Scanned | Merged Stig Bjørlykke Lua Debugger enable/disable-state cleanup. Useful lifecycle/UI-state example, but existing reentrancy/state conventions already cover the durable principle. |
| !24607 | Scanned | Merged Clang Analyzer dead-store cleanup, reviewed/merged by John Thacker. Static-analysis hygiene only; no new general convention. |
| !24606 | Deep | Merged generated registry update. John Thacker questioned special-casing an apparently unassigned BACnet vendor entry; Jaap Keuter preferred reflecting the authoritative organization data as supplied until a stable source convention exists. Useful source-of-truth evidence, but not promoted as a separate rule. |
| !24605 | Scanned | Merged release-4.6 automatic registry/data update. Backport/data maintenance only. |
| !24604 | Scanned | Merged release-4.4 automatic registry/data update. Backport/data maintenance only. |
| !24603 | Scanned | Merged John Thacker GitHub Actions refresh to Node.js 24-era actions and a maintained MSVC command-prompt action fork. CI dependency maintenance. |
| !24602 | Scanned | Merged Qt DIS Stream Analysis follow-up correctly guards QtMultimedia-only code while keeping common plotting logic unconditional. Reinforces optional-dependency compile guards. |
| !24601 | Down-weighted | Closed, unmerged DIS radio-stream-analysis follow-up. Superseded by the clean merged !24602 path; not treated as accepted architecture. |
| !24600 | Scanned | Merged release-4.6 backport of NetScaler wiretap Buffer-length correction. Corroborates existing buffer bookkeeping guidance. |
| !24599 | Scanned | Merged Lua Debugger shortcut/key-handling consolidation. UI behavior only. |
| !24598 | Scanned | Merged release-4.6 backport of DCT2000 wiretap Buffer-length correction. Corroboration only. |
| !24597 | Deep | Merged release-4.4 backport of the UDS non-progress/infinite-loop fix from !24590. Strong corroboration of parser-progress validation. |
| !24596 | Deep | Merged release-4.6 backport of the UDS non-progress/infinite-loop fix from !24590. Strong corroboration of parser-progress validation. |
| !24595 | Deep | Merged John Thacker NetScaler fix uses Buffer-aware append/read helpers so the Buffer's logical length tracks bytes actually written. Strong corroboration of existing source-vs-produced-buffer guidance. |
| !24594 | Scanned | Merged release-4.6 DCT2000 cleanup reducing static arrays. Backport/maintenance only. |
| !24593 | Scanned | Merged GitLab CI Visual Studio Code Analysis repair. CI-specific maintenance without a broader convention. |
| !24592 | Scanned | Merged checker-driven dissector cleanups. Reinforces mechanical repository checks; no distinct new rule. |
| !24591 | Deep | Merged John Thacker DCT2000 wiretap Buffer-length fix. Logical Buffer length must describe materialized bytes, not merely allocated capacity; corroborates existing notebook guidance. |
| !24590 | Deep | Merged John Thacker UDS fix rejects protocol-forbidden zero address/length components before they can produce a zero-progress parsing loop. Promoted to `parser-truncation-dispatch-conventions.md`. |
| !24589 | Scanned | Merged John Thacker RTPproxy conversion to `tvb_get_string_uint*()` helpers removes temporary string copies. Reinforces canonical helper reuse already recorded. |
| !24588 | Deep | Merged John Thacker core reassembly optimization records already-used fragments with `FD_DEFRAGMENTED` so partial-reassembly reset does not copy the same bytes out and back again; enables common reset logic. Strong accepted core architecture/performance evidence, but too implementation-specific for a new standalone rule. |
| !24587 | Scanned | Merged release-4.6 backport of full-file wiretap Buffer growth fix. Corroboration only. |
| !24586 | Scanned | Merged release-4.6 backport accepting protocol-defined zero-length BGP MUP Source Address. Reinforces protocol semantics over generic nonzero assumptions. |
| !24585 | Deep | Merged John Thacker full-file wiretap fix increases Buffer logical length when materializing an entire file. Corroborates existing Buffer-length invariant. |
| !24584 | Scanned | Merged John Thacker removal of a 12-year-unreachable Kerberos 3DES-only libnettle implementation. Good dead-compatibility-code cleanup evidence, but no separate convention added. |
| !24583 | Down-weighted | Closed, unmerged packet-frame color-filter NULL-dereference proposal with detailed reproducer. Useful bug evidence but not treated as accepted implementation because the MR was abandoned/superseded. |
| !24582 | Scanned | Merged Lua Debugger Add Watch usability/deduplication change. UI-only. |
| !24581 | Deep | Merged HTTP/2 GRegex/GMatchInfo ownership repair, validated with sanitizer output, fixing both leaks and a use-after-free. Reinforces existing ownership/refcount conventions rather than adding another overlapping rule. |
| !24580 | Scanned | Merged John Thacker `tvb_get_string_uint8/16` API addition and conversions. Strong helper-reuse/performance idiom already covered by fetch/convert-once guidance. |
| !24579 | Scanned | Merged John Thacker reassembly refactor extracts common fragment free/reset code into static helpers, setting up later !24588 simplification. Refactoring evidence only. |
| !24578 | Scanned | Merged AUTHORS email-address update. No engineering convention. |
| !24577 | Scanned | Merged Lua Debugger paused-line gutter indicator. UI-only. |
| !24576 | Deep | Merged BGP MUP correction accepts Source Address Length 0 because the protocol explicitly defines it as 'address not carried'. Reinforces semantic length validation: zero is invalid only when the protocol says so. |
| !24575 | Deep | Merged John Thacker CMake update uses `LibXml2::LibXml2` imported target so include paths, libraries, and definitions travel together. Strong corroboration of the imported-target convention already in `build-conventions.md`. |
| !24574 | Scanned | Merged John Thacker androiddump linkage cleanup removes libwiretap dependency when the libpcap build path does not need it; udpdump drops an unnecessary wiretap include. Reinforces dependency minimization/layering. |
| !24573 | Scanned | Merged release-4.4 backport making `ANDROIDDUMP_USE_LIBPCAP` visible in the required CMake parent scope. Backport corroboration. |
| !24572 | Scanned | Merged release-4.6 backport of the same androiddump CMake-scope fix. Backport corroboration. |
| !24571 | Scanned | Merged John Thacker CI removal of OpenSUSE 15.6 shortly before upstream support ended, avoiding disproportionate maintenance for an obsolete matrix entry. CI lifecycle maintenance. |
| !24570 | Deep | Merged HTTP URL-encoded percent-decoding fix after John Thacker explicitly rejected broad conversion from reported lengths to captured lengths: ordinary TVB access should throw the canonical bounds exception on truncated captures. The accepted fix was narrowed to the real `%xx` three-byte structural check. Promoted to `parser-truncation-dispatch-conventions.md`. |
| !24569 | Deep | Merged John Thacker master androiddump fix sets the option in the CMake scope where the moved subdirectory can actually observe it and fixes a sign conversion. Build-scope correctness, no new general rule. |
| !24568 | Deep | Merged Lua Debugger Developer's Guide chapter after extensive review. Strong evidence that substantial developer-facing facilities should document architecture, state behavior, limitations, troubleshooting, and usage together; documentation-specific and not promoted separately. |
| !24567 | Deep | Merged Stig Bjørlykke Lua Debugger freeze-main change prevents nested-event-loop re-entry, unsafe teardown, and live-capture entry while Lua is paused. Strong corroboration of the existing `reentrancy-conventions.md` event-source/lifecycle gating rule. |
| !24566 | Deep | Merged John Thacker source-layer cleanup moves packet-provider code into libwireshark, limits extcap support sources to targets that actually support extcap, and keeps CheckAPI coverage on application sources. Reinforces dependency layering and checker coverage. |
| !24565 | Scanned | Merged John Thacker CMake cleanup removes duplicate targets that depended on the same Asciidoctor output. Build-graph simplification only. |
| !24564 | Deep | Merged release-4.6 wiretap safety backport warns when record captured length exceeds Buffer length and assures capacity to prevent overflow while flagging the underlying module bug. Corroborates Buffer metadata/capacity invariants. |
| !24563 | Scanned | Merged release-4.6 pcapng frame change processes Custom Options on every block type that can legally contain options. Protocol-format compliance update. |
| !24562 | Scanned | Merged release-4.4 Buffer assertion backport: logical length must never be increased beyond allocated space. Strong invariant corroboration. |
| !24561 | Scanned | Merged release-4.6 Buffer assertion backport. Same invariant; no additional rule. |
| !24560 | Deep | Merged DECT NR+ PCC-only Feedback fix. Stig Bjørlykke explicitly required deciding in `dissect_dect_nr()` that the variant has no MAC PDU before calling `dissect_mac_pdu()`. Promoted to `parser-truncation-dispatch-conventions.md`. |
| !24559 | Deep | Merged DECT NR+ TAP header dissector after 59 review notes and months of iteration. Review reinforced documenting the format source, using `_ret_` tree helpers instead of duplicate fetches, consistent naming/filter structure, and reviewable protocol registration; these points already exist in `dissector-conventions.md`. |

## Promoted durable conventions

- !24570: TVB bounds exceptions are intentional truncation control flow in ordinary dissection; do not broadly substitute captured-length clipping merely to avoid them. Validate the actual local protocol structure instead.
- !24590, corroborated by !24596/!24597: packet-controlled stride/length values used by loops must be validated so every successful iteration makes progress; reject protocol-forbidden zero before it can become a zero-byte increment.
- !24560: decide whether a nested PDU/structure exists at the caller layer that owns the discriminator, and do not invoke the lower-level decoder when that structure is absent.

Strong merged work in !24567 (reentrancy/lifecycle gating), !24575 (CMake imported targets), !24595/!24591/!24585/!24564/!24562/!24561 (Buffer logical-length/capacity invariants), !24588 (partial-reassembly lifecycle), !24589/!24580/!24559 (`tvb` helper reuse), and !24581 (ownership cleanup) was intentionally treated as corroboration of conventions already present rather than duplicated.
