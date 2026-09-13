# Wireshark MR review batch: !20989–!21038

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base before this run: `681fd55fe84c2803feb050edbb8c1da531c72277`

Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers (including the historical !17571–!17620 batch and the supplemental automation ledger) plus `reviewed-mrs.md`. The fifty highest-numbered corpus MRs not in that set were selected. All corpus files !21038 through !20989 exist, so this batch contains that exact contiguous set. No numeric range was assumed reviewed merely from a neighboring ledger.

Weighting: merged master work and explicit maintainer review were treated as the strongest evidence. Stable-branch/backport-only changes were used mainly as corroboration. Open !21032 was treated as provisional and was not promoted into a durable convention.

| MR | Review depth | Disposition |
|---|---|---|
| !21038 | Scanned | Merged portability/header-name fix; reinforces existing supported-toolchain portability guidance. |
| !21037 | Deep | Merged warning-policy change; explicit Michael Mann + John Thacker agreement that statement-like macros should be invoked as `MACRO();`, with the semicolon outside the macro. Promoted to `c-portability-conventions.md`. |
| !21036 | Scanned | Merged netlog error-path fix; avoid continuing with freed/invalid parsed address state. Existing error-path/lifetime guidance sufficient. |
| !21035 | Scanned | Merged WebSocket heuristic context fix; reinforces existing dissector context/state-contract guidance. |
| !21034 | Scanned | Merged capture-sync validation fix for unsupported link-layer types; no distinct new convention. |
| !21033 | Scanned | Merged IPv6 heuristic refactor; useful separation but no additional durable rule. |
| !21032 | Discussion-focused (open) | HTTP/2/HTTP/3 desegmentation design remains open; useful provisional evidence only, deliberately down-weighted. |
| !21031 | Scanned | Merged documentation correction; no reusable engineering rule. |
| !21030 | Scanned | Merged stable-branch Qt backport; no new convention beyond master behavior. |
| !21029 | Scanned | Merged FetchArtifacts migration; build/tooling maintenance with no distinct review rule. |
| !21028 | Deep | Merged Skinny generator parity repair, approved by Guy Harris; generator/template must reproduce accepted generated output, and Guy recommends stable build targets before CI automation. Promoted to `generated-code-conventions.md`. |
| !21027 | Scanned | Merged Strato manual-page cleanup; documentation-only. |
| !21026 | Scanned | Merged spelling cleanup; no durable engineering lesson. |
| !21025 | Deep | Merged capture-filter UI work with substantive Guy Harris review; exact compiled-filter diagnostics must use the real capture context/backend. Promoted with !21014 to `capture-diagnostic-conventions.md`. |
| !21024 | Scanned | Merged Clang 22 compile fix; narrow compiler cleanup, existing portability guidance sufficient. |
| !21023 | Discussion-focused | Merged capture-sync plumbing supporting the live-context filter work; corroborates !21014/!21025. |
| !21022 | Scanned | Merged Qt Compiled Filter Output error-state presentation; corroborates the capture diagnostic work. |
| !21021 | Scanned | Merged automatic data update; no reusable engineering lesson. |
| !21020 | Scanned | Merged automatic data update; no reusable engineering lesson. |
| !21019 | Scanned | Merged automatic data update; no reusable engineering lesson. |
| !21018 | Discussion-focused | Merged prerequisite cleanup for `-Wextra-semi`; corroborates !21037's macro-semicolon convention. |
| !21017 | Scanned | Merged C12.22 zero-length/NULL `memcpy` fix; directly corroborates the already-recorded C API call-contract rule from later !21282. |
| !21016 | Scanned | Merged documentation/function-name correction; no new convention. |
| !21015 | Scanned | Merged netlog resource cleanup on open failure; reinforces existing ownership/error-path rules. |
| !21014 | Deep | Merged live-capture API for retrieving the actually compiled BPF program; foundation for !21025 and promoted to `capture-diagnostic-conventions.md`. |
| !21013 | Discussion-focused | Merged include-what-you-use/header self-sufficiency fix; reinforces existing public-header validation convention. |
| !21012 | Scanned | Merged cllog bounds fix; reinforces existing parser-boundary guidance. |
| !21011 | Scanned | Merged O-RAN preference rearrangement; no broader rule extracted. |
| !21010 | Discussion-focused | Merged log3gpp parser-state cleanup moves large mutable buffers to per-parser state; corroborates existing reentrancy/state-lifetime guidance. |
| !21009 | Scanned | Merged CMake/man-page maintenance; no durable lesson. |
| !21008 | Deep | Merged John Thacker TLS stream identity work; higher-layer follow/session features should use a stable protocol-layer stream identity rather than infer it from lower transport type. Promoted to `conversation-identity-conventions.md`. |
| !21007 | Scanned | Merged NSIS/Stratoshark packaging update; no reusable engineering rule. |
| !21006 | Scanned | Merged CI removal of obsolete macOS Intel job; infrastructure maintenance. |
| !21005 | Discussion-focused | Merged Qt stale-selection crash fix; event ordering must not expose stale dissection state, but existing state/UI lifetime guidance is sufficient. |
| !21004 | Scanned | Merged iWARP MPA v2 protocol support; no distinct review convention extracted. |
| !21003 | Scanned | Merged CI job addition for wsar; no broader convention extracted. |
| !21002 | Scanned | Merged 6-GHz 802.11 tuning capability fix; domain-specific correctness, no general rule. |
| !21001 | Discussion-focused | Merged Bluetooth vendor-command parameter visibility fix; useful UI/dissection behavior but no new durable convention. |
| !21000 | Discussion-focused | Merged Diameter custom-entity XML correction; preserves an extension point while keeping the base dictionary stable, but no broader rule promoted from this single configuration-specific example. |
| !20999 | Deep | Merged Guy Harris RTP writer cleanup uses shared `pint.h` endian helpers and names byte order as big/little endian rather than swapped/unswapped. Promoted with !20997 to `c-wire-layout-portability-conventions.md`. |
| !20998 | Scanned | Merged Guy Harris documentation update following the `pint.h` rename; corroborates !20997. |
| !20997 | Deep | Merged tree-wide Guy Harris API rename makes unsigned load/store semantics explicit; promoted to `c-wire-layout-portability-conventions.md`. |
| !20996 | Scanned | Merged Falco plugin-version update; dependency maintenance only. |
| !20995 | Scanned | Merged conditional CMake option cleanup for Stratoshark tools; no distinct convention. |
| !20994 | Scanned | Merged Falco event handling skips unsupported list fields; no broader rule. |
| !20993 | Discussion-focused | Merged Aggregation View feature; substantial UI feature but its design discussion did not justify a new cross-cutting convention beyond existing UI/state guidance. |
| !20992 | Scanned | Merged wsjson documentation example; documentation-only. |
| !20991 | Scanned (backport) | release-4.2 SSIZE_MAX Windows backport; corroborates existing portability practice. |
| !20990 | Scanned (backport) | release-4.4 SSIZE_MAX Windows backport; corroborates existing portability practice. |
| !20989 | Scanned | Merged removal of an obsolete experimental HTTP/2 frame type; protocol-maintenance cleanup, no broader convention. |

## Durable notebook changes from this batch

- `generated-code-conventions.md`: generated/template parity plus a stable regeneration target before CI enforcement, from merged !21028 with Guy Harris approval/commentary.
- `c-portability-conventions.md`: statement-like macros omit an embedded trailing semicolon; callers write `MACRO();`, from merged !21037 and prerequisite !21018 with explicit Michael Mann/John Thacker agreement.
- `capture-diagnostic-conventions.md`: authoritative compiled-filter output/validity must use the actual execution context and backend; context unavailable to lightweight background checking should not be flattened into false certainty, from merged !21014/!21025 and substantive Guy Harris review.
- `conversation-identity-conventions.md`: higher-layer follow/session features should persist an explicit protocol-layer stream identity when transport identity is insufficient, from merged !21008 by John Thacker.
- `c-wire-layout-portability-conventions.md`: shared byte-order helpers and API names should make unsigned value-domain and endian semantics explicit rather than use host-relative swapped/unswapped language, from Guy Harris-authored/merged !20997 and !20999.

Exact MR count: **50**.
