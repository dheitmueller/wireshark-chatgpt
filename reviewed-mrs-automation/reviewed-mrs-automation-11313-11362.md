# Automated Wireshark MR review: !11313–!11362

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection method: rebuilt the already-reviewed set from the available per-run ledgers under `reviewed-mrs-automation/` and `reviewed-mrs.md`, preserving/counting the historical `!17571`–`!17620` batch. The prior mention of `!11362` was only a frontier probe. The fifty highest-numbered corpus MRs not in the reviewed set were selected individually; no numeric interval was assumed reviewed merely from partial ledger coverage.

Batch summary: 50 MRs reviewed. 49 merged; 1 closed/unmerged (`!11320`). Merged master work was weighted most heavily, stable backports as corroboration, and the closed proposal was down-weighted. `!11320` was explicitly superseded by a later solution (`!13004`) according to Guy Harris.

| MR | State / review depth | Durable assessment |
| --- | --- | --- |
| !11362 | Merged — deep/corroboration | COPS moves a pointer array retained in file-scoped state to `wmem_array`, so the container itself follows file lifetime instead of leaking a GLib container whose elements alone were scoped. Reinforces allocator/container lifetime guidance. |
| !11361 | Merged — deep/corroboration | Huawei GSM A-bis/RSL paging decoding is corrected using observed vendor data/patent evidence rather than preserving an inferred multi-byte layout. Reinforces using authoritative protocol/vendor evidence over guesses. |
| !11360 | Merged — scanned/tooling | Consolidates allocator-debugging environment setup into a helper file and documents the workflow. Useful developer tooling maintenance; no new architectural rule. |
| !11359 | Merged — scanned | Adds NVMe/TCP TLS dissection support. Useful protocol capability; limited review discussion/sample evidence in the corpus, so not promoted as a durable convention. |
| !11358 | Merged — deep | Replaces runtime parsing of the shipped services registry with generated sorted/static data while retaining personal services data as the user override layer. Promoted with !11315 to generated-registry guidance. |
| !11357 | Merged — scanned | Generated/specification update; no distinct durable convention beyond existing generated-code rules. |
| !11356 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11355 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11354 | Merged — corroboration | Assigned-number/generated-data maintenance; reinforces treating authoritative registry inputs and regeneration as source-of-truth work. |
| !11353 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11352 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11351 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11350 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11349 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11348 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11347 | Merged — scanned | Generated/specification update; no distinct durable convention. |
| !11346 | Merged — deep/corroboration | Stable-branch decompression bounds hardening validates a packet-controlled tree index before writing. Reinforces bounds-checking rules. |
| !11345 | Merged — deep/corroboration | Companion stable-branch decompression bounds hardening; same accepted correctness direction as !11346. |
| !11344 | Merged — documentation | Documents display-filter subtraction tokenization/spacing behavior. Useful syntax documentation, not a broader code convention. |
| !11343 | Merged — deep/corroboration | Opus padding validity is checked against reported packet length so capture truncation is not mislabeled as protocol-invalid padding; captured length remains about byte availability. Strongly reinforces the existing reported-vs-captured-length rule. |
| !11342 | Merged — scanned | Registry/data naming cleanup; no distinct durable convention. |
| !11341 | Merged — deep/corroboration | Removes the obsolete runtime global enterprises text input after generated lookup conversion; discussion reinforces keeping generator invocation and scheduled assigned-number update automation synchronized. Promoted with !11315/!11358. |
| !11340 | Merged — deep/corroboration | Opus output sizing uses the codec/library's sample-count result instead of a fixed assumption. Reinforces deriving buffer/output sizes from the authoritative decoder result. |
| !11339 | Merged — deep/submission | DNS SVCB `dohpath` work was supplemented with a requested representative capture and before/after evidence. Reinforces capture-backed dissector validation. |
| !11338 | Merged — scanned | Spelling/text cleanup; no durable convention. |
| !11337 | Merged — deep | SDP preserves format-specific parameters as opaque negotiated metadata and forwards them to RTP/media-specific owners rather than interpreting codec semantics in SDP. Promoted to callback/context layering guidance. |
| !11336 | Merged — deep/corroboration | DHCP failover Microsoft behavior is supported with captures from distinct implementations and an explicit compatibility preference. Reinforces implementation-diversity testing and conservative interoperability controls. |
| !11335 | Merged — deep/corroboration | QUIC multipath draft support includes concrete capture validation. Protocol feature work with good sample-backed evidence; no separate new rule. |
| !11334 | Merged — deep/testing | Lua tests are reorganized around clearer per-frame expectations/counts and unnecessary setup is removed. Corroborates focused, behavior-oriented tests. |
| !11333 | Merged — deep | RFC 2198 catches nonfatal exceptions from a bounded/known child encoding, reports them locally, restores packet context, advances using parent-owned length metadata, and continues to later siblings; fatal errors still propagate. Promoted to parser-dispatch guidance. |
| !11332 | Merged — deep/corroboration | E2AP nested dissector calls protect/fence outer column state. Reinforces that wrapper dissectors own presentation context around child calls. |
| !11331 | Merged — corroboration | Skinny C99 conversion updates generated/template-side material along with generated output. Reinforces generator/output parity. |
| !11330 | Merged — scanned | ORAN extension-length correctness update; focused protocol fix, no broader convention. |
| !11329 | Merged — deep/corroboration | SDP replaces a bespoke fixed-size hex parser with the common `convert_string_to_hex()` helper and uses its produced length. Reinforces common parsing helpers and avoiding arbitrary scratch limits. |
| !11328 | Merged — scanned | E2AP component request/response subdissector dispatch support; no distinct new convention. |
| !11327 | Merged — scanned | HTTP/3 comment/reference update to final RFC 9218; documentation only. |
| !11326 | Merged — scanned/internal architecture | Display-filter function arguments move to `GPtrArray`/reference counting. Internal representation cleanup; accepted but not generalized beyond existing ownership/container guidance. |
| !11325 | Merged — scanned/internal architecture | Display-filter function results use the register/cell abstraction directly. Internal VM/API cleanup; no separate durable rule extracted. |
| !11324 | Merged — scanned | PDU Transport cleanup/style/ownership simplification; no new convention beyond existing cleanup guidance. |
| !11323 | Merged — scanned | Broad include-consistency cleanup; reinforces header hygiene but adds no new rule. |
| !11322 | Merged — deep/corroboration | Microsoft DHCP failover hostnames require explicit UTF-16 little-endian decoding; reviewer feedback and a sample establish the byte order. Reinforces specifying both character encoding and endianness from wire evidence/specification. |
| !11321 | Merged — deep/corroboration | Stable backport handles a contradiction in the DHCP failover draft by recognizing the deployed value and clearly displaying the compatibility interpretation. Reinforces explicit, documented handling of known specification defects. |
| !11320 | Closed/unmerged — down-weighted | Proposed pcapng `if_fcslen` bit-to-byte conversion. Guy Harris later closed it with “Done differently in !13004.” Treated as superseded negative/history evidence, not as accepted implementation guidance. |
| !11319 | Merged — stable backport | RTCP Slice Loss Indication mask correction backport. Corroborates the accepted master fix in !11317. |
| !11318 | Merged — stable backport | RTCP Slice Loss Indication mask correction backport. Corroborates !11317. |
| !11317 | Merged — master fix | Corrects the RTCP Slice Loss Indication field mask. Focused wire-layout correctness fix; no additional durable convention beyond existing bit-mask guidance. |
| !11316 | Merged — stable backport | PEAP state-key fix backport; corroborates the master change in !11313. |
| !11315 | Merged — deep | Profiling showed runtime parsing of the global enterprise registry consumed about 10.9% of a short-capture TShark run. The accepted generated-static baseline preserves personal/profile runtime overrides and the scheduled registry update workflow. Promoted with !11358 to generated-registry guidance. |
| !11314 | Merged — deep/corroboration | RTP RFC 2198 tap output can expose the de-encapsulated primary payload useful to RTP Player while retaining a preference for raw encapsulated export. Reinforces matching tap/export representation to the consumer's semantic contract while preserving raw access when useful. |
| !11313 | Merged — deep/corroboration | PEAP includes the current protocol/TLS layer in the EAP protocol-data key so multiple TLS layers do not collide. Reinforces complete state identity and is corroborated by stable !11316. |

## Notebook changes from this run

- `parser-truncation-dispatch-conventions.md`: added the rule that a parent may continue after a nested nonfatal decoder failure only when it independently owns reliable sibling boundaries; restore mutable packet context before continuing. Source: merged !11333. Commit: `e1b1b8780c7cd4fdcc9387ee9326e9ffb4ed7e61`.
- `callback-context-conventions.md`: extended negotiated-metadata guidance with the SDP-side rule that opaque consumer-specific parameters should be preserved and forwarded to the layer that owns their semantics. Source: merged !11337, reinforcing !11378/!11410. Commit: `2813f6ff80d7a0711f43c181e56ebd45a3931ef5`.
- `generated-registry-data-conventions.md`: added generated-global-registry guidance covering startup optimization, deterministic update jobs, personal/profile overrides, and preservation of externally useful artifacts. Sources: merged !11315 and !11358, with !11341 workflow corroboration. Commit: `29403596b1e59e38914e24af75b0594eaefabd5a`.
- Existing `dissector-entry-and-state-conventions.md` already records the reported-vs-captured-length distinction, so !11343 is retained here as strong corroboration rather than duplicating that rule.
- Existing allocator/generated-code/testing guidance likewise already covers the durable portions of !11362, !11331, !11339, and related MRs; those are retained as corroborating evidence rather than duplicated.

## Frontier check

`!11312` exists in the corpus at the same corpus commit (`DHCPFO: Accept empty message digest option`, closed/unmerged). It was inspected only to establish that the corpus continues below this batch and is **not** counted as reviewed here. Therefore the corpus is not exhausted and no scraper restart is required after this run.
