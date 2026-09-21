# Wireshark MR Automation Review: !14999–!15048

Corpus commit: `1c7ad509887ee25079a7865cc62ba18cba06f49a`

Reviewed count: **50**

Selection was made by exact set subtraction, not by assuming numeric ranges. Before selecting the batch, the notebook review-tracking inventory was consulted together with `reviewed-mrs.md`, the supplemental automation tracking, and the exact per-run ledgers at the current review frontier. The previously reviewed !17571–!17620 batch remains explicitly counted. The immediately preceding exact ledger covers !15049–!15098, and the corpus commit is unchanged since that run, so the next fifty highest-numbered unreviewed records are exactly !15048 through !14999 inclusive.

Exact reviewed MR set:

`!15048, !15047, !15046, !15045, !15044, !15043, !15042, !15041, !15040, !15039, !15038, !15037, !15036, !15035, !15034, !15033, !15032, !15031, !15030, !15029, !15028, !15027, !15026, !15025, !15024, !15023, !15022, !15021, !15020, !15019, !15018, !15017, !15016, !15015, !15014, !15013, !15012, !15011, !15010, !15009, !15008, !15007, !15006, !15005, !15004, !15003, !15002, !15001, !15000, !14999`

Status weighting: **49 merged, 1 closed/unmerged (!15027)**. Merged master changes were treated as primary evidence; stable backports were mainly corroborating evidence. !15027 was down-weighted as a rejected approach and used only as negative evidence alongside accepted !15026.

## Durable findings promoted to the notebook

- **!15047 / !15048 — validated parser length must propagate to caller progress.** John Thacker's merged master Mongo fix makes callers advance using the effective length returned by `dissect_bson_document()` rather than reusing an untrusted signed BSON length that the helper had already sanitized. Added to `dissector-consumption-boundary-conventions.md`; !15048 and the already reviewed !15049/!15050 stable backports corroborate the master result.
- **!15026 with negative evidence from !15027 — checker rules must model API semantics.** Martin Mathieson's accepted checker correction excludes `proto_tree_add_uint()` from a fixed-width source-span warning because the value is supplied separately from the tvbuff span. Closed !15027 demonstrates the danger of blindly obeying the earlier false positive by changing valid source spans. Added to `checker-target-conventions.md`.
- **!15023 / !15040 — speculative lookup must not create protocol state.** John Thacker's merged JSON-3GPP fix gates HTTP/2 header lookup on actual HTTP/2 presence because the apparently query-like helper creates HTTP/2 session state internally and could contaminate HTTP/1.1 conversations. Added to `dissector-state-conventions.md`.
- **!14999 — manual TCP desegmentation must respect capability/preferences and request the exact known shortfall.** Jaap Keuter explicitly required checking both the VNC desegmentation preference and `pinfo->can_desegment`, and recommended requesting the known number of missing bytes instead of `DESEGMENT_ONE_MORE_SEGMENT` when possible. The accepted change was merged by Anders Broman. Added to `tcp-desegmentation-conventions.md`.

## Corroborating findings retained without duplicate notebook rules

- **!15042 plus !15043–!15045:** Guy Harris's master XZ security-pin warning and its stable copies show the value of documenting known-dangerous dependency versions adjacent to version pins, but this incident-specific maintenance note was not generalized into a new project-wide rule.
- **!15039 / !15038 / !15037 / !15036 / !15041 / !15033:** continued project-owned C99 type and const-correctness cleanup, including Qt callback/signal surfaces. These reinforce existing standard-C-type and const-correctness guidance.
- **!15034:** the checker expansion that exposed the `proto_tree_add_uint()` false positive is useful context for !15026 and demonstrates why checker changes themselves need validation against real API contracts.
- **!15028 plus !15030–!15032 and !15020:** explicit bounded dissection-depth tracking and narrowly justified recursion suppressions reinforce existing recursion-resource guidance.
- **!15029:** separate gRPC dissector handles for native/Web/Web-Text transports reinforce using distinct entry points instead of inferring mode later from incidental `pinfo` fields.
- **!15025:** the editcap `memmove()` correction reinforces deriving copy length from the actual source pointer to the valid end of the buffer.
- **!15018 plus !15021/!15022:** an Illumos-only compile failure shows why unusual conditional-compilation paths benefit from platform/build coverage.
- **!15004 plus !15005–!15007:** bounded non-NUL protocol bytes should be compared with length-aware tvbuff APIs rather than C-string APIs that can scan outside the intended range.
- **!15003:** the contributor supplied a focused pcap plus before/after screenshots and explicitly scoped known unrelated semantic problems in the sample, reinforcing evidence-driven dissector submissions.
- **!15046 / !15002:** I/O Graph changes continue to reinforce semantic invalidation: retap only when a dependency affecting derived data changes, but do retap when the Y-field/filter dependency genuinely changes.

## Per-MR review record

| MR | State | Review note |
|---|---|---|
| !15048 | Merged | release-4.2 backport of Mongo effective-length/offset fix; corroborates !15047. |
| !15047 | Merged | Master Mongo fix; caller now advances by the BSON parser's sanitized returned length. Promoted. |
| !15046 | Merged | I/O Graph recalculation timing avoids invalidating/recomputing when semantic inputs have not changed. |
| !15045 | Merged | Stable backport of the XZ compromised-version warning represented by !15042. |
| !15044 | Merged | Stable backport of the XZ compromised-version warning represented by !15042. |
| !15043 | Merged | Stable backport of the XZ compromised-version warning represented by !15042. |
| !15042 | Merged | Guy Harris-authored master maintenance warning documents a known compromised XZ release next to the pin. High-authority but incident-specific. |
| !15041 | Merged | I/O Graph const-correctness cleanup; corroborates existing API const guidance. |
| !15040 | Merged | release-4.2 backport of JSON-3GPP HTTP/1.1 / HTTP/2 state-creation fix from !15023. |
| !15039 | Merged | Large Qt/UI C99-type conversion; signal/slot type consistency noted during review. |
| !15038 | Merged | Project-owned type migration cleanup; corroborates existing standard-C-type guidance. |
| !15037 | Merged | Project-owned type migration cleanup; corroborates existing standard-C-type guidance. |
| !15036 | Merged | Project-owned type migration cleanup; corroborates existing standard-C-type guidance. |
| !15035 | Merged | Stable HTTP/2 header text-decoding fix uses encoding-aware string conversion rather than treating text as opaque bytes. |
| !15034 | Merged | Extends typed-item checking for fixed-width address fields; later refined by !15026 after false-positive analysis. |
| !15033 | Merged | Const-correctness cleanup for packet-list model access. |
| !15032 | Merged | Stable backport of explicit dissection-depth handling represented by !15028. |
| !15031 | Merged | Stable backport of explicit dissection-depth handling represented by !15028. |
| !15030 | Merged | Stable backport of explicit dissection-depth handling represented by !15028. |
| !15029 | Merged | Stable gRPC fix uses distinct transport-specific dissector handles instead of incidental match-string inference. |
| !15028 | Merged | Gerald Combs master fix adds explicit bounded recursion/depth handling for GQUIC and DOCSIS. Corroborates existing rule. |
| !15027 | Closed | Rejected checker-driven source-span changes; Martin Mathieson explained that `proto_tree_add_uint()` source span can legitimately differ from destination field width. Negative evidence only. |
| !15026 | Merged | Martin Mathieson fixes checker semantics for `proto_tree_add_uint()`; promoted. |
| !15025 | Merged | editcap copy length corrected to match the actual shifted source pointer and remaining valid buffer extent. |
| !15024 | Merged | Static-analysis/Coverity cleanup with no new architectural lesson beyond existing warning-driven review practices. |
| !15023 | Merged | John Thacker JSON-3GPP fix prevents speculative HTTP/2 header lookup from creating false HTTP/2 session state; promoted. |
| !15022 | Merged | Stable backport of cpu_info conditional-platform compile fix from !15018. |
| !15021 | Merged | Stable backport of cpu_info conditional-platform compile fix from !15018. |
| !15020 | Merged | Gerald Combs adds narrowly justified clang-tidy recursion suppressions where separate depth invariants already bound recursion. |
| !15019 | Merged | release-3.6 end-of-series release-note maintenance; no reusable implementation convention. |
| !15018 | Merged | Fixes a missing declaration in an unusual cpu_info conditional-compilation path seen on Illumos; later backported. |
| !15017 | Merged | Asciidoc `manarg` line-splitting documentation formatting adjustment. |
| !15016 | Merged | Modernizes Qt signal/slot connections to typed syntax; maintenance cleanup. |
| !15015 | Merged | Adds automatic profile switching based on display-filter matches; substantial UI feature, but no distinct durable rule promoted from this run. |
| !15014 | Merged | release-3.6 version-preparation maintenance. |
| !15013 | Merged | release-4.0 version-preparation maintenance. |
| !15012 | Merged | release-4.2 version-preparation maintenance. |
| !15011 | Merged | Clang Analyzer dead-store cleanup, including using a parser's returned offset where progress matters; corroborating static-analysis evidence. |
| !15010 | Merged | Build/release maintenance for 3.6.22. |
| !15009 | Merged | Build/release maintenance for 4.0.14. |
| !15008 | Merged | Build/release maintenance for 4.2.4. |
| !15007 | Merged | release-3.6 backport of IPPUSB bounded comparison fix from !15004. |
| !15006 | Merged | release-4.0 backport of IPPUSB bounded comparison fix from !15004. |
| !15005 | Merged | release-4.2 backport of IPPUSB bounded comparison fix from !15004. |
| !15004 | Merged | John Thacker replaces C-string comparison of bounded non-NUL tvbuff bytes with `tvb_memeql`; strong corroboration of bounded-buffer API use. |
| !15003 | Merged | BSSGP Attach Indicator lookup-table correction with focused pcap and before/after screenshots. |
| !15002 | Merged | I/O Graph retaps when Y-field/filter dependencies change; corroborates semantic invalidation guidance. |
| !15001 | Merged | I/O Graph logarithmic axis now uses a logarithmic ticker as required by the plotting library. |
| !15000 | Merged | I/O Graph treats line+point styles semantically as line graphs when deciding whether zero values are meaningful. |
| !14999 | Merged | VNC multi-segment desegmentation fix with substantive Jaap Keuter review on capability/preference gates and exact missing-byte requests. Promoted. |

Notebook commits produced by this run before the ledger commit:

- `66b5fa3b1e747643346e7a06436ac69ea84d2ecf` — validated parser length propagation.
- `74e0d5d840d2c65f0d7c361f638386540bf4bf39` — semantic checker API modeling.
- `34f9a1376db473ad5a7d271554d73f2bdc8aa8b7` — speculative protocol-state creation hazards.
- `fd73213d1a5e62dfe4257848a8f5db87896e98d0` — exact TCP desegmentation requests.
