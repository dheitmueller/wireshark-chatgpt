# Wireshark MR review automation ledger — !12113–!12162

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection was reconstructed from `reviewed-mrs.md`, the per-run ledgers in `reviewed-mrs-automation/`, and supplemental automation tracking. The historical !17571–!17620 batch remains preserved and counted. Numeric interval coverage was not assumed: each candidate was checked against the available tracking. The prior !12162 mention was a frontier probe only, not a completed review.

Exact reviewed set (50 MRs): !12162, !12161, !12160, !12159, !12158, !12157, !12156, !12155, !12154, !12153, !12152, !12151, !12150, !12149, !12148, !12147, !12146, !12145, !12144, !12143, !12142, !12141, !12140, !12139, !12138, !12137, !12136, !12135, !12134, !12133, !12132, !12131, !12130, !12129, !12128, !12127, !12126, !12125, !12124, !12123, !12122, !12121, !12120, !12119, !12118, !12117, !12116, !12115, !12114, !12113

Outcome: 47 merged, 3 closed/unmerged (!12152, !12148, !12114). Closed/superseded work is retained as lower-weight review evidence and is not treated as accepted implementation precedent.

| MR | Outcome | Review depth | Durable evidence / result |
|---|---|---|---|
| !12162 | Merged | Scanned | GitLab CI macOS Arm prefix-path fix on release-3.6; branch-specific build environment maintenance, no new cross-cutting convention. |
| !12161 | Merged | Scanned | Release-4.0 cherry-pick of macOS code-signing update; corroborates platform packaging maintenance. |
| !12160 | Merged | Deep / promoted | Broad conversion from anonymous `create_dissector_handle()` handles to named `register_dissector()` identities so dissectors are discoverable by generic consumers; promoted to initialization lifecycle conventions. |
| !12159 | Merged | Scanned | Release-4.0 macOS CMake flag cleanup; branch-specific build maintenance. |
| !12158 | Merged | Deep / promoted | Companion broad named-dissector registration conversion; establishes registration in `proto_register_*()` and later binding in handoff. |
| !12157 | Merged | Deep / promoted | Guy Harris fixes timestamp-precision range check to use `WS_TSPREC_MAX` rather than negative `TS_PREC_AUTO`; Coverity-detected follow-up to !12133. |
| !12156 | Merged | Deep / promoted | John Thacker changes HTTP file/content data from `FT_STRING`/forced ASCII to `FT_BYTES`; raw content is an octet stream until media semantics define encoding. |
| !12155 | Merged | Scanned | Master macOS CMake flag cleanup; no new durable convention. |
| !12154 | Merged | Scanned | Follow-up 64-bit Z21 format-specifier repair; corroborates existing portable C formatting guidance. |
| !12153 | Merged | Deep / corroboration | Original-author Z21 64-bit formatting fix after !12152 was rejected; discussion exposed another missed macOS format mismatch and motivated stronger API checking. |
| !12152 | Closed / unmerged | Discussion-focused | Rejected/superseded Z21 attempt used `%lld`/`%llu`; John Thacker required `PRId64`/`PRIu64` (or project equivalents) because C data models differ. Superseded by !12153/!12154. |
| !12151 | Merged | Scanned | ASTERIX adjustment for an upstream/generated-item extension change; localized generated-dissector maintenance. |
| !12150 | Merged | Scanned | Accepted Clang Analyzer dead-assignment/increment cleanups; useful static-analysis maintenance, no distinct new convention. |
| !12149 | Merged | Scanned | Clang Analyzer dead-initialization cleanup; straightforward static-analysis maintenance. |
| !12148 | Closed / unmerged | Scanned / down-weighted | Flawfinder 4.1.0 work did not merge; no accepted implementation convention promoted. |
| !12147 | Merged | Scanned | Stable-branch cherry-pick skipping global-name-resolution tests on macOS; platform-specific test-environment maintenance. |
| !12146 | Merged | Scanned | Companion stable-branch cherry-pick for macOS global-name-resolution test skipping. |
| !12145 | Merged | Scanned | Master macOS code-signing update; packaging maintenance. |
| !12144 | Merged | Scanned | Stable-branch cherry-pick moving macOS notarization into GitLab CI; release engineering maintenance. |
| !12143 | Merged | Scanned | Companion stable-branch notarization cherry-pick. |
| !12142 | Merged | Scanned | Master move of macOS notarization to GitLab CI; useful release engineering evidence, no new notebook rule beyond existing platform-build guidance. |
| !12141 | Merged | Deep / testing evidence | Adds sharkd hosts tap together with a dedicated DNS/mDNS capture and JSON-RPC regression test; corroborates API-feature regression testing. |
| !12140 | Merged | Scanned | ITS Collective Perception Service update to newer ETSI material; protocol-spec maintenance. |
| !12139 | Merged | Scanned | Removes an unneeded CMake definition; localized build cleanup. |
| !12138 | Merged | Scanned | Stable-branch cherry-pick of !12130 Windows updater/open-file fix. |
| !12137 | Merged | Deep / corroboration | Protobuf packed repeated fixed32 values must choose decode width from the packed element size, not outer wire type 2; accepted cherry-pick, reinforces semantic-vs-container parsing. |
| !12136 | Merged | Scanned | CFM sender-ID IPv6 typo correction; localized field fix. |
| !12135 | Merged | Deep / corroboration | Martin Mathieson corrects field masks, including ISIS expense mask; reinforces existing mask/value-domain checker guidance. |
| !12134 | Merged | Deep / deferred provenance | IDN backport replaces misuse of conversation options with an explicit full key and frame-indexed configuration history; strong evidence, but original master change is !12110 and is deferred to that review. |
| !12133 | Merged | Deep / promoted | Guy Harris evolves recent-file timestamp precision to accept legacy symbolic names plus extensible numeric values while writing old names when possible for backward compatibility; promoted with !12157. |
| !12132 | Merged | Scanned | Stable-branch cherry-pick of !12131 IEEE 1722 reserved-byte fix. |
| !12131 | Merged | Deep / corroboration | IEEE 1722 PCM parser now exposes the reserved byte it previously skipped, fixing incomplete-dissection accounting; localized correctness/backport-worthy fix. |
| !12130 | Merged | Deep / UI evidence | John Thacker adds an explicit software-update file-close context so unsaved-state policy is handled by the common close path without spawning an unexpected nested dialog; useful UI state-machine evidence. |
| !12129 | Merged | Scanned | Automatic data/update regeneration for 2023-09-17; generated maintenance only. |
| !12128 | Merged | Scanned | Automatic data/update regeneration for 2023-09-17; generated maintenance only. |
| !12127 | Merged | Scanned | Automatic data/update regeneration for 2023-09-17; generated maintenance only. |
| !12126 | Merged | Scanned | dumpcap migration to refactored logging APIs; API-maintenance change. |
| !12125 | Merged | Deep / corroboration | ASN.1-generated `FT_UINT64` security-level field uses `val64_string`/`VALS64`; reinforces matching value-table width/API to field type. |
| !12124 | Merged | Scanned | WSDG Windows Qt installation documentation update. |
| !12123 | Merged | Scanned | WSDG uses Bourne-shell syntax for UNIX-family environment-variable examples; documentation portability cleanup. |
| !12122 | Merged | Deep / UI evidence | John Thacker asks the active Qt style for line-edit content size instead of assuming font-metric width, fixing Adwaita without regressing other styles; retained as GUI evidence. |
| !12121 | Merged | Scanned | WSDG environment-variable documentation clarification. |
| !12120 | Merged | Scanned | WSDG fixed-width formatting for setup-script names. |
| !12119 | Merged | Scanned | WSDG AsciiDoc heading-spacing cleanup. |
| !12118 | Merged | Scanned | WSDG UNIX-family subsection organization cleanup. |
| !12117 | Merged | Discussion-focused / caution | RFC 9072 BGP support; John Thacker later noted that a newly 16-bit optional-parameter length must not remain stored in `guint8`. Kept as review caution rather than accepted convention because the captured comment was unresolved. |
| !12116 | Merged | Deep / corroboration | John Thacker fixes repeated Windows stats opens by closing/resetting the reusable named-pipe handle, propagating creation failure, and logging errors; reinforces explicit resource lifecycle/error propagation. |
| !12115 | Merged | Scanned | Spelling corrections; no durable convention. |
| !12114 | Closed / unmerged | Scanned / superseded | Earlier spelling-fix attempt closed; down-weighted relative to merged !12115. |
| !12113 | Merged | Deep / corroboration | E2AP converts a silent table-capacity TODO into packet-local expert information and tightens missing-OID handling; reinforces expert diagnostics for packet-visible protocol limits. |

## Notebook updates promoted in this run

- `initialization-lifecycle-conventions.md`: !12158 and !12160 establish that reusable/discoverable dissectors should receive stable named identities with `register_dissector()` during protocol registration; handoff should bind the already-registered handle to tables/preferences rather than creating an anonymous handle there.
- `field-value-semantics-conventions.md`: !12156 establishes that encoding-neutral/raw payload belongs in `FT_BYTES`; a framing layer must not invent text encoding when semantic decoding belongs to a media/subdissector layer.
- `configuration-serialization-conventions.md`: Guy Harris's !12133 plus Coverity-driven !12157 establish a backward-compatible persisted-enum pattern: accept legacy symbolic spellings plus an extensible numeric domain, preserve old spellings where useful to older readers, and keep sentinels distinct from ordinary range bounds.

Strong corroboration intentionally not duplicated into new notebook rules includes the portable 64-bit formatting sequence !12152–!12154, IDN historical conversation/configuration state in !12134 (master original !12110 remains for the next batch), Qt style-aware sizing in !12122, reusable OS-handle teardown/error propagation in !12116, mask corrections in !12135, and value-table width matching in !12125.

Frontier check only (not reviewed): !12112, `wsutil: Rewrite ws_return macros`, exists in the corpus and is merged. It is the next descending candidate absent newly scraped higher-numbered unreviewed material.
