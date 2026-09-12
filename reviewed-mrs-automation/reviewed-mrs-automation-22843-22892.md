# Automated MR review ledger: !22843–!22892

Reviewed using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base commit before this run: `37a2fa5c4e49f8fa13208f2f8424b194ee3d9d77`

Exactly 50 previously unreviewed merge requests were reviewed in this run. Selection was built from the individually recorded MR numbers in all available files under `reviewed-mrs-automation/` plus `reviewed-mrs.md`; numeric ranges were not treated as reviewed merely because a ledger happened to cover part of the range. The previously reviewed !17571–!17620 batch remains explicitly preserved and counted. The fifty highest-numbered corpus MRs not already in that reviewed set were !22892 through !22843, all of which are present in the corpus.

Merged MRs were treated as stronger evidence than abandoned/superseded MRs. Maintainer-authored or maintainer-reviewed changes were weighted more heavily, with especially authoritative reviewer guidance promoted only when the accepted implementation supported it.

| MR | Outcome | Depth | Review result |
|---|---|---|---|
| !22892 | merged | Deep/corroboration | RTPS backport bounds header-extension parsing with a subset TVB. Strong reinforcement for bounding nested parsers with TVBuff structure rather than trusting inconsistent implicit lengths; already covered by parser/bounds guidance. |
| !22891 | merged | Deep/corroboration | Adds `tvb_find_uint8_remaining()` boolean-status APIs with unsigned offset output so “not found” is not encoded as `-1` in an otherwise unsigned domain. Strong corroboration of existing API-domain/sentinel guidance. |
| !22890 | merged | Scanned | SMB/SMB2 access-mask corrections and additions; protocol-specific semantic accuracy, no new general convention. |
| !22889 | merged | Corroboration | H.265 stable-branch fix preserves non-truncating division required by the specification before applying `ceil`; reinforces existing arithmetic/specification-exactness guidance. |
| !22888 | merged | Corroboration | H.265 stable-branch counterpart of the integer-division correction; same evidence as !22886/!22889. |
| !22887 | merged | Corroboration | Continues TVBuff uncompress APIs toward unsigned offsets/lengths; reinforces established semantic integer-domain guidance. |
| !22886 | merged | Deep/corroboration | H.265 master fix casts before division because the specification explicitly requires mathematical division before ceiling. Useful reminder that host-language integer semantics must not silently alter normative formulas; existing arithmetic guidance covers it. |
| !22885 | merged | Scanned | Restores executable bits on Python tools; repository/tooling hygiene, no distinct durable rule. |
| !22884 | merged | Deep/corroboration | S1AP TRY/CATCH offset fix distinguishes the volatile/dissected offset from the value restored by exception handling. Reinforces existing parser-coordinate and exception-flow guidance. |
| !22883 | merged | Corroboration | DCERPC-MAPI records `start_offset` after alignment so range accounting refers to the actual field start. Reinforces existing parser/protocol-item coordinate guidance. |
| !22882 | merged | Scanned | NTLMSSP restores OEM decoding required by MS-NLMP despite Unicode negotiation flags; protocol-spec-specific correctness and a useful anti-regression comment, but no broader rule needed. |
| !22881 | merged | Deep/corroboration | LZNT1 avoids unsigned underflow at output offset zero and tightens unsigned variable domains. Reinforces checked arithmetic and boundary-base-case guidance. |
| !22880 | merged | Scanned | Automatic registry/translation update with DMX failure noted; generated-data maintenance only. |
| !22879 | merged | Scanned | Automatic registry/translation/data update; no durable convention. |
| !22878 | merged | Scanned | Automatic registry/translation/data update; no durable convention. |
| !22877 | merged | Deep/promoted | John Thacker fixes `rawshark` handling of POSIX `rlim_t`, whose unsigned semantic contract does not imply 32-bit width. The prior `uint32_t *` type-pun could partially overwrite a wider object and fail on big-endian systems. Promoted to `c-type-conventions.md`: size/convert platform typedefs according to their actual contract, using typed temporaries and normal assignment rather than pointer-casting through an assumed width. |
| !22876 | merged | Deep/corroboration | Request/response header parsing only asks line-end parsing to signal desegmentation when the buffer is not capture-truncated. Reinforces existing truncation-vs-reassembly semantics. |
| !22875 | merged | Scanned | nl80211 nested-attribute support; protocol/platform feature work without reusable review guidance. |
| !22874 | merged | Scanned | Falco event API update for falcosecurity-libs 0.22; dependency adaptation only. |
| !22873 | merged | Deep/negative evidence | John Thacker documents that flexible-array-member layout tricks are not legal C++ and rewrites one particularly dubious address computation, while explicitly noting the surrounding representation remains unsafe. Kept as cautionary evidence rather than promoted as an accepted architecture pattern. |
| !22872 | merged | Deep/corroboration | John Thacker keeps plugins loaded while running under Valgrind because `dlclose()` prevents useful address-to-symbol translation. Useful instrumentation-specific practice; recorded here but not elevated to a general runtime-lifetime rule. |
| !22871 | merged | Deep/corroboration | PCNFSD allocation fix ensures the deobfuscation result is NUL-terminated before string operations. Reinforces existing bounded-string/NUL-termination guidance. |
| !22870 | merged | Deep/corroboration | PCNFSD deobfuscation cleanup fixes missing termination before `strlen` and makes identity/password handling consistent. Same existing string-boundary lesson as !22871. |
| !22869 | merged | Scanned | NATS registers `max_msgs` as a 64-bit integer so numeric filtering has correct semantics; protocol-field type correctness, no distinct new convention. |
| !22868 | merged | Corroboration | Qt checks `dynamic_cast` results even where failure is not expected and removes a needless preceding C-style cast. Reinforces existing checked-cast/type-safety practice. |
| !22867 | merged | Corroboration | RTPS uses `tvb_memdup()` into packet scope instead of `tvb_get_ptr()` + GLib duplication + manual free. Reinforces scope-aware ownership and TVBuff-native helper usage. |
| !22866 | merged | Deep/corroboration | Master RTPS subset-TVBuff memory-safety fix corresponding to !22892; protocol-declared boundaries should be encoded structurally so inconsistent implicit lengths cannot parse past the extension. Existing bounded-parser guidance already captures this. |
| !22865 | merged | Corroboration | QCustomPlot adaptive-sampling fix on another supported branch; same GUI sampling correction as !22860/!22864, no general rule promoted. |
| !22864 | merged | Corroboration | QCustomPlot adaptive-sampling fix in another branch/path; no separate convention. |
| !22863 | merged | Scanned | Bluetooth ATT timeout analysis support; feature-specific state tracking, no general convention extracted. |
| !22862 | merged | Deep/corroboration | MRD verifies the address family before applying IPv4-specific address tests. Reinforces validating tagged/variant data before interpreting representation-specific bytes. |
| !22861 | merged | Deep/corroboration | Diameter flags and returns on impossible too-short AVP lengths rather than continuing with invalid derived lengths. Reinforces existing structural-length validation and progress rules. |
| !22860 | merged | Corroboration | QCustomPlot adaptive-sampling correction; GUI-library-specific fix, no new general convention. |
| !22859 | merged | Deep/corroboration | New CITP dissector discussion explicitly recognizes that a dissector may be invoked multiple times for one packet, so call-count-based conversation state was removed. Strong reinforcement for arbitrary redissection/idempotent state guidance already in the notebook. |
| !22858 | merged | Scanned | O-RAN tap read the wrong tap-structure member; straightforward correctness fix. |
| !22857 | merged | Deep/corroboration | Removes plugin-only globals after P4RPC became built-in because duplicate definitions triggered ASan ODR violations. Reinforces keeping build-mode scaffolding aligned with the actual linkage model. |
| !22856 | merged | Corroboration | Uses `proto_tree_add_item_ret_length()` for FT_STRINGZ instead of separately scanning with `tvb_strnlen()`. Reinforces fetch/derive-once helper guidance already in `dissector-conventions.md`. |
| !22855 | merged | Scanned | Script-warning cleanup; tooling hygiene, no distinct convention. |
| !22854 | merged | Scanned | GSM SIM Terminal Capability support and tag/length field separation; protocol-specific field modeling. |
| !22853 | closed, unmerged draft | Down-weighted | Experimented with making whitespace failures visually louder in CI. Pipeline failed and the draft was eventually closed; no accepted convention promoted. |
| !22852 | merged | Scanned | Adds LS ELECTRIC XGT FEnet dissector. Reviewed as a protocol feature; no durable cross-cutting convention identified. |
| !22851 | merged | Deep/corroboration | Converts more TVBuff APIs to unsigned offsets where negative-offset semantics are not supported. Reinforces the existing semantic unsigned-domain migration. |
| !22850 | merged | Deep/corroboration | BPv7 tap structures expose durable block-length data rather than requiring tap consumers to inspect a TVB that is no longer valid at that use point. Reinforces existing lifetime/ownership rules: publish durable data across lifecycle boundaries rather than transient packet objects. |
| !22849 | merged | Scanned | DHCPv6 permits RFC 9686 message types to carry Client FQDN, downgrades the prior error, and continues decoding. Protocol-spec update, no general rule beyond existing expert-severity/continue-when-decodable practice. |
| !22848 | merged | Scanned | O-RAN tap/UI reporting counts symbols exceeding expected timing; feature-specific analysis. |
| !22847 | merged | Deep/promoted | New ESUN dissector initially claimed IEEE Local Experimental EtherType `0x88B5`. John Thacker explicitly rejected automatic public registration on a shared experimental value, requested `dissector_add_for_decode_as()`, and noted fixed registration can follow a final IEEE assignment. The contributor implemented that design and John merged it. Promoted to `dissector-registration-conventions.md`. |
| !22846 | closed, superseded | Down-weighted | Adds one SCSI header include, but John Thacker noted !22844 had already implemented the needed fix; closed without merge. No independent evidence promoted. |
| !22845 | merged | Corroboration | Converts `tvb_strsize` / `tvb_get_stringz_enc` offsets and lengths to unsigned semantics and updates callers. Reinforces existing API-domain guidance. |
| !22844 | merged | Deep/corroboration | John Thacker adds the authoritative SCSI header includes where those typedefs are used; !22846 was superseded by this accepted change. Reinforces existing public-header/dependency hygiene. |
| !22843 | merged | Corroboration | asn2wrs generates unsigned offsets as part of the broader TVBuff offset-domain migration. Reinforces existing generated-code/API-domain guidance. |

## Durable notebook updates

1. **Platform typedef width and representation** — `c-type-conventions.md` now records !22877: standards/platform typedefs such as `rlim_t` must be handled according to their actual width and semantic contract. Parse/convert through correctly typed temporaries and assign normally rather than type-punning the object's address through a guessed fixed-width pointer.
2. **Shared experimental discriminator registration** — `dissector-registration-conventions.md` now records !22847: public Wireshark builds should not automatically claim local/experimental protocol codepoints that are intentionally reusable. Offer Decode As until an authoritative unique assignment exists.

## Strong evidence intentionally not duplicated

- !22892/!22866 reinforce bounded subset-TVBuff parsing.
- !22891/!22887/!22851/!22845/!22843 reinforce unsigned offset domains and status-plus-output APIs instead of negative sentinels.
- !22881 reinforces checked arithmetic and explicit zero/base cases.
- !22876 reinforces truncation-aware desegmentation.
- !22867 reinforces packet-scope ownership and TVBuff-native copy helpers.
- !22859 reinforces arbitrary redissection/idempotent state handling.
- !22850 reinforces lifetime-safe data transfer to delayed consumers.
- !22844, while accepted, merely reinforces header self-sufficiency/dependency hygiene; superseded !22846 was explicitly down-weighted.
