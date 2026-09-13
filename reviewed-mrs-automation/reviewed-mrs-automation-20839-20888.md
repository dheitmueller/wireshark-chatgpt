# Automated Wireshark MR review — !20839 through !20888

Corpus revision: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers and sparse `reviewed-mrs.md` tracking, preserving and counting the historical !17571-!17620 batch. The previous run ended at !20889. All fifty corpus IDs !20888 down through !20839 are present, and no pre-existing tracking entry was found for any of them, so these are the fifty highest-numbered previously unreviewed MRs in this corpus revision.

Weighting: merged master MRs and direct maintainer-authored/reviewed rationale receive strongest weight. Stable-branch backports provide useful confirmation of accepted behavior. Closed, open, draft, abandoned, or superseded work is retained in the audit set but down-weighted as precedent.

## Exact reviewed set

- !20888 — Scanned, merged. WSDG documentation for clang-format; useful contributor documentation, no new durable convention.
- !20887 — Scanned, merged. CPython dependency update; no durable Wireshark-specific lesson.
- !20886 — Discussion-focused, merged; Martin Mathieson. Dissector-warning fixes and checker work around field width/masks; corroborates existing field/checker semantics.
- !20885 — Scanned, merged. LIX2 update to newer 3GPP TS 33.128 revision; protocol-specific.
- !20884 — Discussion-focused, merged; John Thacker. Replaces integer channel-type parameters with the domain enum; corroborates typed-domain API guidance.
- !20883 — Discussion-focused, merged. Adds/document `json_get_int`; review catches implicit `gint64` to `int` narrowing and requires an explicit representation choice.
- !20882 — Scanned, merged; Gerald Combs. Falco plugin source-name mapping; no separate durable rule.
- !20881 — Scanned, merged. O-RAN ST10/ST11 section-count update; protocol-specific.
- !20880 — Discussion-focused, merged. BLF CAN-XL support with capture/testing discussion; later John Thacker lifetime note strongly corroborates use of `pinfo->pool`/`tvb_memdup` for packet-owned copied data.
- !20879 — Deep, merged; Michael Mann. Large conversion from ambient `wmem_packet_scope()` to explicit allocator-aware value-string APIs; strongly corroborates allocator-scope guidance.
- !20878 — Discussion-focused, merged. RTPS AES256/PSK corrections with representative captures; also corroborates explicit packet allocator ownership.
- !20877 — Discussion-focused, open. Qt welcome-page capture-filter persistence work; down-weighted because not merged.
- !20876 — Scanned, merged; Martin Mathieson. O-RAN field-condition and field-semantics corrections; no new general rule.
- !20875 — Deep, merged; Michael Mann. Renames allocator-aware `val_to_str_ext_wmem` into the primary API, making allocator scope explicit instead of hidden; corroborates allocator-scope guidance.
- !20874 — Scanned, merged. WSLua `base.SPECIAL_VALS` support and tests; useful API/test coverage, no separate convention.
- !20873 — Deep, merged. Extcap sanitizes invalid preference-name characters. Direct discussion distinguishes developer-defined preference names, where an assertion can expose a programming bug, from user/extcap-supplied names, which must be handled recoverably. Promoted to `assertion-static-analysis-conventions.md`.
- !20872 — Scanned, merged. O-RAN beam ID parsing cleanup; protocol-specific.
- !20871 — Deep, merged; authored by John Thacker. `-T ek` now emits typed values according to Elasticsearch schema rather than inheriting UI hexadecimal/text formatting. Promoted to `machine-output-conventions.md`.
- !20870 — Scanned, merged. vcpkg export/version/checksum update; build/dependency maintenance only.
- !20869 — Scanned, merged. Plot automatic scrolling support; UI feature, no general convention.
- !20868 — Discussion-focused, merged. mergecap option to suppress capture comments; review reinforces that option documentation should explain behavior rather than read as a bug report.
- !20867 — Scanned, merged; Michael Mann. actrace parsing cleanup using combined proto-tree return-value helpers; corroborates avoiding duplicate extraction.
- !20866 — Deep, merged; Michael Mann. Broad allocator-aware `val_to_str_ext` conversion; corroborates explicit allocator scopes.
- !20865 — Discussion-focused, merged; Michael Mann. Generated-dissector allocator conversion plus generator fixes; corroborates generated-output source-of-truth/reproducibility guidance.
- !20864 — Discussion-focused, merged; Michael Mann. ASN.1 generated-dissector allocator conversion; corroborates generated source reproducibility and explicit allocator scopes.
- !20863 — Discussion-focused, merged; Michael Mann. DCE/RPC generated/non-generated allocator conversion; accepted successor to the closed attempts below.
- !20862 — Discussion-focused, closed/superseded. Earlier DCE/RPC generated-dissector allocator conversion exposed files not regenerated cleanly from source; down-weighted because replaced by !20863.
- !20861 — Discussion-focused, closed/superseded. Combined generated-dissector allocator conversion failed regeneration checks; Martin Mathieson and John Thacker identified generated-output diffs. Strong corroboration of existing generator/source-of-truth guidance, but not itself accepted precedent.
- !20860 — Deep, merged stable backport; authored/merged by John Thacker. Unknown sFlow records are shown as opaque data, diagnosed, and skipped by their full padded length so later records remain synchronized. Promoted to `parser-boundary-conventions.md`.
- !20859 — Scanned, merged stable backport; John Thacker. Correct 802.11 center-frequency calculation reused from iw-derived logic; implementation-specific.
- !20858 — Scanned, merged stable backport; John Thacker. Zigbee Direct path tolerates absent IEEE/MAC hints when relayed over BLE; defensive nullable-context fix.
- !20857 — Scanned, merged. gsm_sim uses the top-level tree for subdissectors, fixing a Coverity finding; no new convention.
- !20856 — Scanned, merged; authored by John Thacker. Adds EHT 320 MHz monitor-mode capability handling; feature-specific.
- !20855 — Scanned, merged; approved/merged by John Thacker. JSON file probing allocates according to actual bounded file size rather than always reserving the maximum; sensible resource-sizing corroboration.
- !20854 — Scanned, closed/superseded. Earlier version of !20855 with a flawed/unfinished formulation; down-weighted.
- !20853 — Discussion-focused, merged; Michael Mann. Removes the last direct dissector `wmem_packet_scope()` call outside value-string helpers and regenerates IDL users; corroborates explicit allocator ownership.
- !20852 — Scanned, merged. HSFZ tester-address correction; protocol-specific.
- !20851 — Scanned, merged stable backport; authored by John Thacker. LZ77 long match length must be read as the specification's 32-bit value; corroborates exact field-width/specification guidance.
- !20850 — Scanned, merged stable backport; authored by John Thacker. Same LZ77 32-bit match-length correction for another maintained branch.
- !20849 — Discussion-focused, merged master; authored by John Thacker. Original LZ77 32-bit match-length fix, with stable backports !20850/!20851; strong corroboration of parser field-width correctness.
- !20848 — Discussion-focused, merged; reviewed/merged by Martin Mathieson. New BIST OUCH dissector with sample capture and extensive iterative review; reinforces providing representative captures and using established value-string/heuristic idioms.
- !20847 — Scanned, merged. Typo/spelling corrections in docs/source; no durable rule.
- !20846 — Scanned, merged. gsm_sim P1/P2 dissection improvements; protocol-specific.
- !20845 — Scanned, merged stable backport; Gerald Combs. Debian libssh dependency transition with compatibility fallback; packaging maintenance.
- !20844 — Discussion-focused, merged. Persists name-resolution changes made through the View menu. Jaap Keuter explicitly requested a descriptive subsystem-prefixed commit message rather than an issue-reference-only message; corroborates existing submission guidance.
- !20843 — Scanned, merged stable backport. Debian asn2deb switches from obsolete snacc to maintained esnacc; packaging maintenance.
- !20842 — Scanned, merged stable backport. Same asn2deb dependency/tool transition for another maintained branch.
- !20841 — Scanned, merged; Gerald Combs. Portable C struct initialization fix plus protocol-name grepability cleanup; portability maintenance.
- !20840 — Deep, merged stable backport; authored/merged by John Thacker. Decode-As entries can intentionally have a NULL current handle after the user selects no dissection; reporting code must honor that valid state rather than assert. Corroborates assertion/precondition guidance.
- !20839 — Discussion-focused, merged. gsm_sim SEARCH RECORD parsing corrected to the applicable specification after explicit consideration of differences between ISO 7816-4 and ETSI TS 102.221; protocol-specific specification work.

## Durable notebook changes from this run

- `parser-boundary-conventions.md` — unknown but length-delimited records must consume/skip their complete encoded extent, including alignment, so following siblings remain synchronized; based on merged John Thacker MR !20860.
- `assertion-static-analysis-conventions.md` — distinguish developer-controlled invalid registrations from externally/user-supplied identifiers; external values must be validated/normalized rather than allowed to trigger programmer assertions, based on merged !20873 and its direct review discussion.
- `machine-output-conventions.md` — new notebook file: typed machine-output modes serialize according to the downstream schema rather than human display formatting, based on merged John Thacker MR !20871.

Existing notebook guidance was strongly corroborated rather than duplicated for explicit allocator scopes (!20879, !20875, !20866, !20864, !20863, !20853), generated-output reproducibility (!20865, !20864, and the superseded !20861/!20862), exact parser field widths (!20849-!20851), representative capture testing (!20848, !20878, !20880), and assertion/precondition handling (!20840).