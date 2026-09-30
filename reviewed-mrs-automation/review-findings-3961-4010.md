# Review findings: !3961–!4010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed, descending from !4010 through !3961 after exact-membership reconciliation. Merged work is weighted above abandoned/superseded work, and substantive maintainer-authored or maintainer-reviewed changes—especially Guy Harris guidance—receive higher weight.

| MR | Outcome | Depth | Findings |
|---|---|---|---|
| !4010 | merged | Scan | Gerald Combs CI change prints package sizes and hashes for release artifacts; observability only. |
| !4009 | merged | Scan / high-authority | Guy Harris typo-only Wiretap cleanup; no durable rule. |
| !4008 | merged | Deep / promoted | Explicit allocator scopes for TVB helpers; Guy Harris requires renaming APIs whose old names still implied packet scope. |
| !4007 | merged | Medium | Release export uses CI_COMMIT_SHA when available and stashes only when necessary; CI source-of-truth corroboration. |
| !4006 | merged | Medium / corroboration | Known bad CRC is reported as decrypt-failure cause instead of a misleading MIC failure. |
| !4005 | merged | Scan | 3.5.0 release maintenance. |
| !4004 | merged | Scan | Bluetooth channel-index wording. |
| !4003 | merged | Medium / corroboration | Further removal of ambient wmem_packet_scope. |
| !4002 | merged | Discussion-focused | Anders Broman requires ENC_NA for aggregate FT_NONE/header item; byte-order encoding belongs on typed scalar fields. |
| !4001 | merged | Backport | Stable 3.2 backport of SMB Export Objects Unicode/output-boundary fix from !3993. |
| !4000 | merged | Backport | Release-3.4 backport of !3993. |
| !3999 | merged | Scan | IEC104 qualifier dissection. |
| !3998 | merged | Scan | GSM SIM offsets/optional-field corrections. |
| !3997 | merged | Scan | IEC104 CP56Time2a field. |
| !3996 | merged | Deep / high-authority | Guy Harris centralizes uint32 option extraction, alignment-safe copy, and endian conversion in common pcapng helper. |
| !3995 | merged | Scan / high-authority | Guy Harris removes stale pcapng code. |
| !3994 | merged | Deep / high-authority | Guy Harris exports common pcapng option processors and makes option byte-order domain explicit. |
| !3993 | merged | Deep / promoted | John Thacker removes lossy SMB ASCII filename canonicalization; rely on central save-time Export Objects filename policy. |
| !3992 | merged | Scan | ISOBUS description correction. |
| !3991 | closed | Down-weighted | Duplicate/superseded ISOBUS submission. |
| !3990 | merged | Deep / high-authority / promoted | BBLog pcapng support with extensive Guy Harris review: cite defining sources, separate pcapng envelope endianness from opaque payload rules, and prefer common callback-driven extension machinery. |
| !3989 | closed | Down-weighted | Duplicate/superseded ISOBUS submission. |
| !3988 | merged | Scan | ASCII apostrophe in guide filenames for Windows/Okular compatibility. |
| !3987 | closed | Down-weighted | Duplicate/superseded ISOBUS submission. |
| !3986 | merged | Discussion-focused | Roland Knall prefers native QString comparison instead of std::string/UTF-8 round-trip; safer for special characters. |
| !3985 | merged | Scan | Release-note correction. |
| !3984 | closed | Down-weighted | Superseded predecessor of !3986. |
| !3983 | merged | Scan | GSM SIM GET IDENTITY/GET DATA. |
| !3982 | merged | Scan | EPL payload-length correction. |
| !3981 | merged | Medium / high-authority | Guy Harris adds CMake uninstall target using BSD-licensed reusable code. |
| !3980 | merged | Medium / high-authority | Guy Harris documents MSVC limitation: imported shared-library data-symbol addresses are not static-initializer constants in plugins. |
| !3979 | merged | Scan | Stable CI publication of Windows PDBs. |
| !3978 | merged | Scan | CI macOS package path fix. |
| !3977 | merged | Deep / promoted | John Thacker restores saved can_desegment before nested AMQP dispatch so tcp_dissect_pdus retains TCP reassembly capability. |
| !3976 | merged | Deep / promoted | Distinct iWARP MPA port/endpoint identity prevents nested RPC state from overwriting outer MPA conversation. |
| !3975 | merged | Scan | 3.2 version bump. |
| !3974 | merged | Scan | 3.4 version bump. |
| !3973 | merged | Scan | CI package path correction. |
| !3972 | merged | Scan | Publish Windows PDBs. |
| !3971 | merged | Medium | New Extreme EXOS internal capture header dissector; no substantive reusable review discussion. |
| !3970 | merged | Scan | Extreme EDP flags/warning cleanup. |
| !3969 | merged | Scan | Build 3.2.16. |
| !3968 | merged | Scan | Build 3.4.8. |
| !3967 | merged | Medium | Compile HTTP3 helper only with required AEAD feature; optional-feature build hygiene. |
| !3966 | merged | Medium | PROFINET stops inventing control semantics for reserved data that caused false malformed frames. |
| !3965 | closed | Down-weighted | Superseded predecessor of !3966. |
| !3964 | merged | Scan | IS-IS Flexible Algorithm fixes. |
| !3963 | merged | Scan | F1AP export-PDU support; Pascal Quantin catches commit-message typo. |
| !3962 | merged | Scan / high-authority | Guy Harris cppcheck unused-variable cleanup. |
| !3961 | merged | Scan | Daily CI build fix; Gerald Combs approval. |

## High-value synthesis

- **!4008** makes allocator lifetime explicit across TVB helper APIs; Guy Harris also requires API names to stop implying packet scope once that lifetime is caller-selected.
- **!3990, !3994, !3996** form a strong early pcapng extension sequence: document private formats, separate standardized framing byte order from extension payload rules, share option machinery, and centralize extraction/endian handling.
- **!3993** plus stable !4000/!4001 preserves Unicode export-object names and defers filename safety to the central output layer.
- **!3977** is the master-origin AMQP example of restoring caller-owned desegmentation state; !4012/!4013 are later stable corroboration.
- **!3976** prevents nested RPC from stealing the outer MPA conversation by giving the carrier a distinct endpoint domain.
- **!4002** reinforces type-appropriate `ENC_*` use; **!3986** preserves Qt string semantics by comparing in QString space; **!3980** records a concrete MSVC plugin/static-data ABI constraint.

No VANC packet type or SMPTE 291 payload family appeared in this batch.
