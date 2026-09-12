# Automated Wireshark MR review: !22292-!22341

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: reconstructed the already-reviewed set from all available per-run files in `reviewed-mrs-automation/` plus `reviewed-mrs.md`, preserving and counting the historical !17571-!17620 batch. The 50 highest-numbered corpus MRs not already recorded as reviewed were then selected individually. For this run the resulting set happens to be the contiguous range !22341 through !22292; the range was not assumed reviewed or unreviewed as a unit.

Exactly 50 MRs were reviewed in this run, newest to oldest:

| MR | Review | Notes |
|---:|---|---|
| !22341 | Reviewed | CoAP/SGP32 URI-Path registration backport; merged, straightforward protocol registration. |
| !22340 | Reviewed | CoAP URI-Path dissector-table support; merged. Useful extension-point work, but no new rule beyond existing registration/dispatch guidance. |
| !22339 | Reviewed | Makes protocol names easier for source grep and updates `README.dissector`; merged. Reinforces keeping examples/documentation aligned with source-shape conventions used by tooling. |
| !22338 | Reviewed | `-Wformat-truncation` warning cleanup; merged. Notes compiler-warning variation with optimization; corroborative only. |
| !22337 | Reviewed | `tools/check_common.py` excludes binary files returned by Git; merged checker robustness cleanup. |
| !22336 | Reviewed | Spelling/generated UI-source cleanup; merged, no additional durable convention. |
| !22335 | Deep | Funnel GSList conversion; merged, but later discussion reports an ASan double-free during Lua plugin reload. Retained as lifecycle/ownership caution rather than promoted as exemplary merged code. |
| !22334 | Reviewed | X.509 certificate-validity expert information; merged. Discussion distinguishes expert diagnostics from optional generated/filterable state. |
| !22333 | Deep | Funnel operations are not taps for CLI applications; merged. Strongly corroborates application ownership of funnel lifecycle rather than packet-tap semantics. |
| !22332 | Deep | John Thacker-authored MS/TP TVBuff cleanup; merged. Preserves semantic reported length, validates before subtraction, and dissects as much truncated input as possible. |
| !22331 | Reviewed | Guy Harris-authored routine-name typo fix; merged, no durable rule. |
| !22330 | Deep | Michael Mann-authored funnel refactor removes application/UI lifecycle operations from tap registration; merged. Added application-layer boundary guidance. |
| !22329 | Reviewed | Registration-order fix for funnel/tap initialization; merged, but architectural !22330 is stronger evidence. |
| !22328 | Reviewed | Removes obsolete tvb_composite member after refactor; merged cleanup. |
| !22327 | Reviewed | Generator sanitizes quoted enterprise names from upstream registry; merged. Corroborates generated-data input sanitation. |
| !22326 | Reviewed | `.mailmap` maintenance; merged, no durable engineering rule. |
| !22325 | Deep | John Thacker-authored HTTP/3 fix caps decoded header output at 1 MiB to resist compression-bomb behavior; merged. Added resource-limit guidance. |
| !22324 | Reviewed | Release-4.4 backport of scalable/non-recursive composite TVBuff work; merged. |
| !22323 | Deep | John Thacker-authored ELF OSS-Fuzz fix validates class/encoding and avoids signed-promotion overflow; merged. Strong arithmetic/provenance corroboration. |
| !22322 | Reviewed | Automatic registry/data update for release-4.6; merged, no additional rule. |
| !22321 | Reviewed | Automatic registry/data update for release-4.4; merged, no additional rule. |
| !22320 | Reviewed | Automatic registry/data update on master; merged. Discussion led to enterprise-name sanitation in !22327. |
| !22319 | Reviewed | DHCPv4 option 162 support with sample/malformed capture evidence; merged. Corroborates capture-backed protocol validation. |
| !22318 | Reviewed | Release-4.6 backport of scalable/non-recursive composite TVBuff work; merged. |
| !22317 | Reviewed | Fixes incorrect pointer passed to LTP subdissector API; merged. |
| !22316 | Reviewed | Release-4.6 backport storing negative nghttp3 errors in a signed type; merged. |
| !22315 | Reviewed | Release-4.6 backport fixes per-header highlighted compressed length; merged. |
| !22314 | Discussion-focused (closed) | SOME/IP dynamic service/method filter-field proposal; closed unmerged with no substantive acceptance evidence, so down-weighted. |
| !22313 | Deep | John Thacker-authored master fix stores negative nghttp3 error codes in signed storage matching the external API domain; merged. |
| !22312 | Deep | John Thacker-authored HTTP/3 fix uses bytes actually consumed for individual compressed-header length; merged. |
| !22311 | Reviewed | John Thacker-authored fix removes accidental double offset increment; merged. |
| !22310 | Deep | John Thacker-authored tvb_composite redesign uses `GSequence` for O(log N) lookup and iterative copying to avoid recursive stack limits; merged. Explicitly distinguishes scalability improvements from the need for aggregate decompression/resource limits. |
| !22309 | Discussion-focused (closed) | SMB FID subtree proposal; Jaap Keuter explicitly rejected mutable static dissector state because random packet access and redissection make temporal invocation order unpredictable. Closed unmerged, but the maintainer architectural constraint was promoted to `dissector-state-conventions.md`. |
| !22308 | Reviewed | John Thacker-authored X.509/X.500 UTCTime conversion to `FT_ABSOLUTE_TIME` where the governing standards define an unambiguous 1950-2049 interpretation; merged. Reinforces semantic field typing. |
| !22307 | Reviewed | Release-4.6 NAS EPS info-column fix; merged backport. |
| !22306 | Reviewed | O-RAN FH CUS SE24 reserved-bit update to `posMeas`; merged protocol update. |
| !22305 | Deep | John Thacker-authored OSS-Fuzz fix validates the legal millisecond domain before converting to nanoseconds, preventing signed overflow; merged. Corroborates validation-before-arithmetic guidance. |
| !22304 | Reviewed | NAS EPS avoids reporting “Ciphered message” after successful deciphering; merged after targeted maintainer agreement. |
| !22303 | Reviewed | O-RAN FH CUS adds ST9 field; merged protocol update. |
| !22302 | Reviewed | BLF validates year before nanosecond conversion to prevent overflow; merged and approved by John Thacker. Corroborates checked arithmetic. |
| !22301 | Reviewed | O-RAN FH CUS adds per-PRB SINR subtrees; merged protocol presentation improvement. |
| !22300 | Reviewed | Release-4.6 backport disables experimental DNS-over-DTLS default binding that conflicts with DNS-over-QUIC while retaining Decode As; merged. |
| !22299 | Reviewed | Release-4.6 backport adds recent DTLS-SRTP protection profiles; merged registry/protocol update. |
| !22298 | Reviewed | Master change disables experimental DNS-over-DTLS default binding in favor of DNS-over-QUIC while retaining Decode As; merged. |
| !22297 | Reviewed | Adds recent DTLS-SRTP protection profiles from current assignments/RFCs; merged. |
| !22296 | Discussion-focused / merged | New PSN dissector underwent substantial maintainer cleanup before merge, including compiler-required block scoping. Useful submission evidence but no distinct new notebook rule. |
| !22295 | Deep | Lua bulk field-request API remained under review for months and was ultimately merged. Reinforces that explicit field requests are part of Wireshark's lazy tree-population model. |
| !22294 | Deep | Lua TreeItem navigation API, extensively revised and ultimately merged. Structural tree navigation operates on fields that were actually requested/populated; useful API semantic evidence, but not promoted as a separate general rule this run. |
| !22293 | Reviewed | Gerald Combs-authored CI change restores macOS build failures as gating after runner reliability improved; merged. |
| !22292 | Reviewed | BER constrained-bitstring size check excludes padding from semantic bit length; merged fix. |

## Durable notebook updates

1. Added `dissector-state-conventions.md`: dissector correctness must not depend on packet visitation order or mutable static “current/previous” state. Wireshark can randomly access and redisect packets; persistent state must be keyed and lifetime-managed appropriately. Primary evidence is Jaap Keuter's explicit review rejection in closed !22309, treated as authoritative negative-design evidence rather than as an accepted implementation.
2. Extended `input-resource-limit-conventions.md`: optimizing parser structure (for example, non-recursive copying and O(log N) lookup in merged !22310) does not replace an explicit aggregate-output/resource ceiling. Merged !22325's 1 MiB HTTP/3 decoded-header cap is the primary accepted evidence.
3. Extended `application-layer-boundary-conventions.md`: application/UI lifecycle callbacks such as funnel initialization should not be modeled as packet-analysis taps merely to obtain callback timing. Merged !22330 and !22333 provide the accepted architecture.

Merged fixes were weighted more heavily than closed/superseded work. Closed !22314 was down-weighted. Closed !22309 was used only for the explicit maintainer architectural rejection, not as evidence that its proposed implementation was accepted. Post-merge regression discussion on !22335 was retained as negative lifecycle/ownership evidence rather than allowing merge status alone to overstate confidence.
