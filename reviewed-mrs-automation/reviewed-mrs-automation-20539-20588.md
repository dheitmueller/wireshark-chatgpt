# Automated MR review ledger: !20539–!20588

Corpus revision reviewed: `dheitmueller/wireshark-corpus-mrs@9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run files under `reviewed-mrs-automation/`; preserved and counted the historical !17571–!17620 batch. The previous descending run ended at !20589. Searches for !20539 and !20588 found no prior notebook review entry, and all 50 corpus objects !20539 through !20588 exist, including explicitly verified !20543. This run therefore reviewed exactly those 50 MRs, newest to oldest.

Weighting: merged master changes and substantive maintainer discussion receive the most weight; stable backports corroborate the corresponding master change; closed/superseded work is recorded but not treated as accepted precedent. Guy Harris technical feedback is treated as especially authoritative.

| MR | Review depth / disposition | Notes |
|---:|---|---|
| !20588 | Merged master — scanned | `proto.c` hash tables switch from GLib string hashing to wmem/xxHash-backed hashing. Performance implementation; no new project-wide rule. |
| !20587 | Merged master — deep | John Thacker moves OID/MIB diagnostics to `ws_log` and avoids constructing expensive strings unless the log message is active. Promoted to `logging-conventions.md`. |
| !20586 | Merged master — scanned | Uses transport-specific port display types so service-name presentation matches UDP/TCP semantics. Useful field-model cleanup. |
| !20585 | Merged master — scanned | IPv6 address-resolution hashing uses xxHash when available with fallback. Performance/portability maintenance. |
| !20584 | Merged master — discussion-focused | wmem string hashing uses seeded XXH3 when available; John Thacker caught a missing `return` during review. Narrow correctness/performance fix. |
| !20583 | Merged master — scanned | Adds Juniper PFCP vendor IE decoding. Protocol-specific extension; no new general convention. |
| !20582 | Merged master — scanned | Additional dissector-warning cleanup. Reinforces warning-clean submission practice. |
| !20581 | Merged master — scanned | Uses `XXH3_64bits_withSeed()` for strong wmem hashing. Internal performance implementation. |
| !20580 | Merged master — scanned | Kafka LZ4 checksum path uses xxHash while preserving the protocol's historical checksum semantics. Protocol-specific compatibility detail. |
| !20579 | Merged master — scanned | PostgreSQL dead-store cleanup found by Clang Analyzer. Static-analysis maintenance. |
| !20578 | Merged master — scanned | `.mailmap` maintenance. No coding convention. |
| !20577 | Merged master — scanned | Installs the xxHash runtime DLL for MSYS2 packaging. Build/packaging maintenance. |
| !20576 | Merged master — scanned | macOS setup SHA256 update. Build-data maintenance only. |
| !20575 | Merged master — deep | Accepted successor to closed !20572. Roon request/reply tracking and ServiceID decoding; supplied a sample pcap. John Thacker argued an unrelated source-file split was premature and should be a separate MR; author folded it back. Corroborates focused-MR and sample-capture guidance. |
| !20574 | Merged master — deep, Guy Harris review | LINKTYPE_LIN padding/length semantics prompted extended discussion because the published layout was ambiguous. Guy distinguished what the text actually specified from an inference based on 4-octet grouping and sought clarification from the format provenance. Promoted to `protocol-validation-conventions.md`. |
| !20573 | Merged release/version maintenance — scanned | Version bump after 4.4.8. No reusable review lesson. |
| !20572 | Closed/superseded — down-weighted | Earlier Roon Discovery update; conflicts/canceled pipeline and replaced by accepted !20575. Not treated as precedent. |
| !20571 | Merged release maintenance — scanned | Builds 4.4.8. No reusable convention. |
| !20570 | Merged master — scanned | Adds xxHash 0.8.3 to CMake/macOS dependency handling. Build maintenance. |
| !20569 | Merged stable backport — corroborating | IAKerb Kerberos error-code update backport; represented by master !20557. |
| !20568 | Merged master — scanned | Adds xxHash development packages to platform setup scripts. Build maintenance. |
| !20567 | Merged master — deep, John Thacker | Decode-As-visible Git/EDHOC media-type handles receive unique names/descriptions. Promoted to new `decode-as-conventions.md`. |
| !20566 | Merged stable backport — corroborating | DLT storage-handle registration-order fix; corroborates master !20563. |
| !20565 | Merged release-4.2 — corroborating | DLT storage-handle registration-order fix backport; corroborates master !20563. |
| !20564 | Merged master — discussion-focused | Adds LDANeo trailer heuristic with validation/timestamp handling. Review exposed Windows integer-width/portability concerns after merge; useful platform-hygiene example but no new standalone rule. |
| !20563 | Merged master — deep, John Thacker | Registers the DLT storage handle only after its owning storage protocol ID exists; otherwise the handle was associated with protocol 0. Promoted to `initialization-lifecycle-conventions.md`. |
| !20562 | Merged master — corroborating | Makes CMP/OCSP/PKIXTSP media-type dissector descriptions unique for Decode As. Strong corroboration for !20567. |
| !20561 | Merged master — scanned | VMware heartbeat version recognition update. Protocol-data maintenance. |
| !20560 | Merged master — discussion-focused | Adds explicit post-force-kill extcap cleanup for Windows so ETW resources are not leaked when `TerminateProcess` bypasses normal shutdown; also fixes zero-valued log-level parsing and extcap-option handling. Platform/process-lifecycle evidence, but not promoted beyond existing lifecycle guidance. |
| !20559 | Merged master — deep, Guy Harris authored/merged | MQ application type is used only to infer byte order and is no longer misused as character encoding; semantic properties are determined and carried independently. Promoted to `api-domain-conventions.md`. |
| !20558 | Merged master — scanned | Adds Juniper CP ID PFCP vendor IE. Protocol-specific extension. |
| !20557 | Merged master — scanned | Adds IAKerb error codes to Kerberos ASN.1 sources. Generated/protocol-data update. |
| !20556 | Merged master — scanned | Corrects byte highlighting for Netlink attribute padding using the actual payload/padding boundary. UI/parser-offset correctness example. |
| !20555 | Merged master — scanned | Caches the full unmasked hash in `wmem_map` entries to reduce repeated hash work. Internal data-structure optimization. |
| !20554 | Merged master — scanned | Dead-store cleanup found by Clang Analyzer. Static-analysis maintenance. |
| !20553 | Merged master — scanned | Fixes additional `check_typed_item_calls.py` dissector warnings. Reinforces project validation scripts. |
| !20552 | Merged master — scanned | Makes XML DTD declarations conform more closely to XML processing-instruction syntax. Specification/data maintenance. |
| !20551 | Merged stable/build maintenance — scanned | Windows GnuTLS 3.8.10 dependency update. No new convention. |
| !20550 | Merged build maintenance — scanned | macOS GnuTLS 3.8.10 dependency update. No new convention. |
| !20549 | Merged master — scanned | GnuTLS dependency/update umbrella change. Build maintenance. |
| !20548 | Merged master — scanned | Extends BLF header/channel parsing for otherwise unparsed payloads. Wiretap feature work; no separate durable rule extracted. |
| !20547 | Merged master — scanned | NSIS missing-space fix. Packaging typo only. |
| !20546 | Closed draft — down-weighted | Experimental `g_str_hash()` replacement using xxHash; abandoned draft and therefore not treated as accepted API precedent. |
| !20545 | Merged master — scanned | NFS `why_no_delegation4` variants now consume their trailing Boolean fields. Protocol-layout correctness. |
| !20544 | Merged master — scanned | MATE Single Copy String handling corrected for huge strings/truncation behavior. Boundary robustness example. |
| !20543 | Merged master — scanned | O-RAN U-plane state records the C-plane frame that supplied `udCompHdr` and exposes the provenance frame in dissection. Corroborates explicit cross-packet state provenance. |
| !20542 | Merged master — scanned | PostgreSQL dead-increment cleanup from Clang Analyzer. Static-analysis maintenance. |
| !20541 | Merged master — scanned | NSIS warning updated to recommend native Arm64 package. Packaging/UI text only. |
| !20540 | Merged master — scanned | Bluetooth data-generation tool retrieves data then writes the output directly. Tool simplification; no new convention. |
| !20539 | Merged master — deep, John Thacker | Service-name cache key now includes both numeric port and transport `port_type`, preventing hits or cached misses from one transport contaminating another. Promoted to `conversation-identity-conventions.md` as a general complete-key identity rule. |

## Durable notebook changes from this run

- `logging-conventions.md`: use the project logging framework and guard expensive diagnostic construction when the log domain/level is inactive (!20587).
- `decode-as-conventions.md`: Decode-As-visible dissector handles need stable unique human descriptions when multiple choices share a protocol (!20567, corroborated by !20562).
- `initialization-lifecycle-conventions.md`: register a dissector handle only after its owning `proto_*` ID has been registered (!20563; stable corroboration !20565/!20566).
- `api-domain-conventions.md`: do not reuse a protocol property as a proxy for a different semantic domain; byte order and character encoding must remain distinct (!20559, Guy Harris).
- `protocol-validation-conventions.md`: ambiguous layout/specification presentation is not sufficient evidence for a stricter wire-format requirement; verify authoritative intent/interoperability before tightening parsing/writing (!20574, Guy Harris).
- `conversation-identity-conventions.md`: cache/state keys must contain every discriminator that changes lookup identity, including transport type for service-name resolution (!20539).

No update to `reviewed-mrs.md` was required; this exact per-run ledger is authoritative for this batch.