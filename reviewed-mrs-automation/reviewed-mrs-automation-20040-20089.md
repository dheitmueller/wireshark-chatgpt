# Wireshark MR review automation ledger: !20040-!20089

## Corpus provenance

- Repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`
- Direction: newest available unreviewed MR toward older MRs.
- Selection method: rebuilt the already-reviewed set from all available per-run files in `reviewed-mrs-automation/` plus `reviewed-mrs.md` where applicable; preserved and counted the historical !17571-!17620 batch; selected the fifty highest-numbered corpus MRs not present in that exact reviewed-ID set. No numeric range was assumed reviewed merely because some IDs in it appeared in prior ledgers.
- Number reviewed this run: **50**.

## Exact MRs reviewed

!20089, !20088, !20087, !20086, !20085, !20084, !20083, !20082, !20081, !20080,
!20079, !20078, !20077, !20076, !20075, !20074, !20073, !20072, !20071, !20070,
!20069, !20068, !20067, !20066, !20065, !20064, !20063, !20062, !20061, !20060,
!20059, !20058, !20057, !20056, !20055, !20054, !20053, !20052, !20051, !20050,
!20049, !20048, !20047, !20046, !20045, !20044, !20043, !20042, !20041, !20040.

## Review notes

| MR | Disposition | Durable result |
|---|---|---|
| !20089 | Scanned, merged | GitLab CI path repair; no additive convention. |
| !20088 | Deep, merged | IEEE 802.11 decryption must tolerate legitimately absent address metadata on alternate encapsulations; defensive entry-point corroboration. |
| !20087 | Scanned, merged | Conversation-filter documentation; no additive convention. |
| !20086 | Scanned, merged | Release-note preparation; no convention. |
| !20085 | Scanned, merged | Release-note preparation; no convention. |
| !20084 | Scanned, merged | Documentation backport; no convention. |
| !20083 | Scanned, merged | macOS dependency refresh; maintenance only. |
| !20082 | Scanned, merged | Windows dependency refresh; maintenance only. |
| !20081 | Reviewed, merged | CMake artifact downloads check status and fail explicitly before extraction; corroborates build failure-contract guidance. |
| !20080 | Deep, merged | `FT_STRING` still requires a character encoding; `ENC_NA` is not appropriate merely because byte order is irrelevant. Strong Guy Harris review evidence; consistent with prior FT_STRING cleanup evidence. |
| !20079 | Deep, merged | Guy Harris-authored pcapng refactor centralizes generic block header/footer framing; corroborates existing pcapng extension architecture guidance. |
| !20078 | Scanned, merged | Qt null-dereference fix; no new convention. |
| !20077 | Scanned, merged | AUTHORS/mailmap maintenance; no new convention. |
| !20076 | Reviewed, merged | Windows/MSYS2 compiler warning cleanup; portability corroboration only. |
| !20075 | Deep, merged | Guy Harris-authored SMB parsing fix checks remaining-byte invariants before mutating offsets/counts and uses canonical byte-count macros; corroborates bounded-parser guidance. |
| !20074 | Deep, merged | Diameter parser migration preserves required local external entities while explicitly prohibiting network retrieval; semantic-equivalence and path-sensitive CI validation promoted to XML parser-security guidance. |
| !20073 | Scanned, merged | Replaces EtherType magic numbers with canonical constants; helper/constant reuse corroboration. |
| !20072 | Scanned, merged backport | Backport of Logcat pointer check; weighted below master fix !20067. |
| !20071 | Scanned, merged backport | Backport of Logcat pointer check; weighted below master fix !20067. |
| !20070 | Reviewed, merged | Cross-build Lemon selection avoids assuming host template availability; build portability corroboration. |
| !20069 | Reviewed, merged | Moves one-time Qt codec discovery to MainWindow; lifecycle/ownership corroboration only. |
| !20068 | Scanned, merged | Coverity-driven variable initialization; narrow fix. |
| !20067 | Reviewed, merged | Null return from token search is diagnosed as malformed input and parsing recovers safely; parser defensive-check corroboration. |
| !20066 | Deep, merged | Dissector documentation separates registration/handoff lifetime from packet work and recommends `pinfo->pool` for temporary dissection memory; corroborates existing allocator/lifecycle guidance. |
| !20065 | Reviewed, merged | Windows static builds model import/export behavior with an explicit compile-time state; platform-build corroboration. |
| !20064 | Reviewed, merged | Encoding cleanup; corroborates !20080 and prior field-encoding evidence. |
| !20063 | Reviewed, merged | Qt5/Qt6 locale conversion compatibility cleanup; no new durable rule. |
| !20062 | Deep, merged | Contributor pipelines may lack privileged project macOS/Windows runners; core-developer rebase can exercise platform jobs absent from contributor namespace. Useful CI/submission context, not promoted as a general coding rule. |
| !20061 | Deep, merged with high-authority unresolved review | Guy Harris identifies the root contract issue: a helper returning success must not leave its output unset; existing helper-success-contract notebook rule already covers this, so no duplicate rule added. |
| !20060 | Scanned, merged | Bluetooth assigned-number/dissection update; protocol-specific. |
| !20059 | Scanned, closed/unmerged | Documentation proposal; deliberately down-weighted. |
| !20058 | Deep, merged | ATT transaction identity includes bearer CID because request/response pairs are scoped to a bearer; strongly corroborates existing state-key identity guidance. |
| !20057 | Reviewed, merged release backport | Capinfos memory-leak fix backport; weighted below !20053 master fix. |
| !20056 | Reviewed, merged release backport | Capinfos memory-leak fix backport; weighted below !20053 master fix. |
| !20055 | Deep, merged | Guy Harris-authored type/format cleanup and printf-like helper removes temporary formatting buffer; API simplification corroboration. |
| !20054 | Scanned, merged | Documentation/release automation marker; no coding convention. |
| !20053 | Reviewed, merged | Static analysis catches missing free in capinfos; ordinary ownership cleanup. |
| !20052 | Reviewed, merged | Analyzer-driven parser offset validation; malformed offsets produce Expert Info instead of invalid state progression. |
| !20051 | Deep, merged | Guy Harris-authored project-wide replacement of hand-rolled alignment arithmetic with `WS_ROUNDUP_n()` / `WS_PADDING_TO_n()`; strongly corroborates already-recorded canonical alignment-helper rule. |
| !20050 | Reviewed, merged | ORAN state change later exposed possible null dereferences in scan-build; reinforces post-merge static-analysis value but no new rule. |
| !20049 | Scanned, merged | Generated ETSI ASN.1/spec update; no additive convention. |
| !20048 | Reviewed, merged | NFS CB_NOTIFY feature; protocol-specific. |
| !20047 | Reviewed, merged | TECMP interface-name behavior and cleanup; protocol-specific. |
| !20046 | Deep, merged | STUN heuristic broadening uses the magic cookie as a stronger discriminator and validates against prior ambiguous captures; heuristic-classification corroboration. |
| !20045 | Scanned, closed/unmerged | Empty-file submission; deliberately down-weighted. |
| !20044 | Deep, merged | Guy Harris-authored correction pins X11 generator input to a known-good release tag and records generator provenance; promoted to source-provenance guidance. |
| !20043 | Deep, merged but superseded in part | Initial X11 regeneration from upstream development input; input choice corrected by !20044, so !20044 receives greater weight. |
| !20042 | Deep, merged | SSH variable-length hostkey array consumes all entries and validates decoded bytes against declared length; bounded/consumed-length parser corroboration. |
| !20041 | Reviewed, merged | Replaces duplicated hex conversion logic with `ws_xton`; canonical helper reuse corroboration. |
| !20040 | Deep, merged | SSH derives a checked payload extent, rejects impossible padding arithmetic with checked subtraction, and parses variable arrays through a bounded subset TVB; strongly corroborates bounded-container and arithmetic-safety guidance. |

## Notebook updates made by this run

- `xml-parsing-security-conventions.md`: added the rule that formats requiring local external entities should retain only the narrowly required local resolver behavior while explicitly disabling network retrieval, plus parser-migration equivalence/path testing guidance from merged !20074.
- `source-provenance-conventions.md`: added generated-input revision pinning and provenance guidance from Guy Harris-authored/merged !20044, treating it as the correction to !20043's less-stable upstream-tip choice.

Other strong findings were retained as corroboration rather than duplicated: !20080 (FT_STRING encoding), !20075/!20042/!20040 (bounded parsing), !20061 (helper success/output contract), !20058 (state-key identity), and !20051/!20041 (canonical project helpers).