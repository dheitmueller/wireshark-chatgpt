# Wireshark MR review automation ledger: !20589–!20638

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run files in `reviewed-mrs-automation/`. The separately reviewed historical batch !17571–!17620 remains part of the reviewed set. The 50 highest-numbered corpus MRs not present in that combined tracking set were then checked individually for corpus presence. Every ID from !20638 through !20589 exists at the pinned corpus commit, so this run contains exactly that contiguous set; no missing-corpus IDs had to be skipped.

Review weighting: merged master changes and substantive maintainer discussion were weighted most strongly; stable backports were treated mainly as corroboration of their master changes; open/draft, closed, abandoned, or superseded MRs were down-weighted. Inline review from highly authoritative maintainers such as Guy Harris was given correspondingly high weight.

## Exact reviewed set

| MR | State / depth | Review result |
|---|---|---|
| !20638 | Merged master — deep | Bluetooth duplicate preferences-module registration removed. John Thacker-authored/merged evidence that registration helpers/upgrades must not cause the same logical prefs module to be registered twice. Promoted to `registration-side-effect-conventions.md`. |
| !20637 | Merged master — scanned | Qt documentation/version compatibility plus guard against a packet-list header sizing crash on older Qt. Useful compatibility fix; no broader convention promoted. |
| !20636 | Merged master — deep | BLF writer/interface mapping fix. Guy Harris explicitly challenged hard-wired `WTAP_ENCAP_ETHERNET` and requested the actual `mand_data->wtap_encap`; discussion resolved before merge. Promoted to `wiretap-interface-metadata-conventions.md`. |
| !20635 | Closed — discussion-focused, down-weighted | Attempt to make xxhash a PRIVATE CMake link dependency. John Thacker closed it after recognizing compiler/linker cases where function-pointer optimization could still require the transitive dependency. Not accepted precedent. |
| !20634 | Merged master — scanned | `wmem_map` item allocation changed to chunked storage/reuse for fewer allocations and better cache behavior. Implementation-specific performance work; no new general notebook rule. |
| !20633 | Merged release-4.4 — scanned | Backport of !20632 RDM Product Detail IDs. No additional lesson beyond master change. |
| !20632 | Merged master — scanned | Adds RDM Product Detail ID constants/value strings from E1.20. Straightforward specification coverage; no new general convention. |
| !20631 | Merged master — scanned | ROON Discovery local helpers made `static`. Straightforward symbol-scope cleanup. |
| !20630 | Merged master — scanned | Small extcap example/documentation reference update. No reusable engineering lesson. |
| !20629 | Closed — down-weighted | First missing-`break` extcap fix attempt; closed almost immediately and superseded by merged !20627. Not implementation precedent. |
| !20628 | Merged master — discussion-focused | Adds several Juniper PFCP IEs after iterative review. Useful dissector-extension example but no sufficiently distinct durable rule beyond existing field/spec conventions. |
| !20627 | Merged master — scanned | Accepted extcap missing-`break` fix; explains why the fallthrough had been masked by option-processing order. Reinforces compiler-warning/static-analysis hygiene already captured elsewhere. |
| !20626 | Merged master — discussion-focused | CMake install logic changed to honor divergent include/library install roots such as Nix layouts. Useful packaging portability evidence; no new Wireshark-specific convention promoted. |
| !20625 | Merged release-4.4 — scanned | libssh 0.11.2 backport with conflict resolution. Backport/dependency maintenance only. |
| !20624 | Merged master — scanned | DNP3 reassembly presentation associates application dissection with the appropriate transport segment/frame. Useful reassembly correctness example; existing reassembly/state guidance already covers the general principle. |
| !20623 | Merged master — scanned | Runs `convert-proto-init.py` for static expert fields in silabs-dch. Mechanical follow-up to the initialization change represented by !20611. |
| !20622 | Merged master — scanned | Column code honors the existing negative `proto_registrar_get_id_byname()` not-found sentinel instead of assuming zero. Reinforces existing API-domain/sentinel guidance. |
| !20621 | Merged master — scanned | QUIC multipath draft-15 update. Specification maintenance without new review convention. |
| !20620 | Merged master — discussion-focused | Adds Geneve Cilium Service Option and includes a focused capture in the MR description. Corroborates existing sample-capture validation guidance. |
| !20619 | Merged master — scanned | Registration path avoids creating/waiting on worker threads when no progress callback exists (e.g. TShark). Good startup optimization, but no new durable rule beyond avoiding infrastructure with no consumer. |
| !20618 | Merged master — scanned | Windows/macOS libssh 0.11.2 dependency update. No reusable convention. |
| !20617 | Merged master — scanned | Qt packet-list drag-label leak fix. Straightforward ownership cleanup. |
| !20616 | Merged release-4.2 — scanned | CI compiler update to clang 20. Build-maintenance change; no new convention. |
| !20615 | Merged master — deep | Removes duplicate OID name registration because `register_ber_oid_dissector()` already registers the name; preserves the previously effective final name where duplicates differed. Promoted with !20613/!20638 to `registration-side-effect-conventions.md`. |
| !20614 | Closed release backport — discussion-focused, down-weighted | Large DNP3 release-4.4 cherry-pick series was not merged as submitted. Useful context only; not accepted stable-branch precedent. |
| !20613 | Merged master — deep | Removes redundant `oid_add_from_string()` calls already performed by `register_ber_oid_dissector()`. Independent evidence for understanding registration-helper side effects; promoted. |
| !20612 | Merged master — scanned | Runs initialization conversion for SICK CoLA expert fields. Mechanical follow-up to !20611. |
| !20611 | Merged master — deep | Defines `EI_INIT` as zero to match current registration initialization and reduce static initialization work while retaining a usable initializer macro for dynamic/Lua cases. Core change; downstream conversion MRs corroborate adoption. No separate notebook rule beyond existing initialization/API contracts was necessary. |
| !20610 | Merged master — scanned | Runs `convert-proto-init.py` for newly introduced PFCP static hfids. Mechanical follow-up. |
| !20609 | Merged master — scanned | PFCP display-filter typo correction. No broader lesson. |
| !20608 | Merged master — discussion-focused | Geneve class IDs refreshed from IANA/RFC 8926 material. Specification/reference maintenance; no new general rule. |
| !20607 | Merged master — scanned | Automatic manufacturer/services/enterprise-number/translation update. No review convention. |
| !20606 | Merged release-4.2 — scanned | Automatic data/translation update. No review convention. |
| !20605 | Merged release-4.4 — scanned | Automatic data/translation update. No review convention. |
| !20604 | Merged master — deep | Fixes OID registration errors caused by swapped string parameters and accidental adjacent-string concatenation; explicitly favors static integer-array OIDs for compile-time constants for both safety and startup cost. Promoted to `registration-side-effect-conventions.md`. |
| !20603 | Merged master — scanned | BMP static hfid initialization converted to zero. Mechanical adoption of current initializer convention. |
| !20602 | Merged master — scanned | Adds tap-listener finish callback so `io_users_t` is released during tap/epan cleanup. Good lifecycle cleanup; existing ownership/lifecycle guidance is sufficient. |
| !20601 | Merged master — discussion-focused | Dynamic column `hf_id` initialization aligned with the zero-based registration convention. The associated follow-up !20622 preserves the existing negative lookup sentinel. Useful API-transition context, but no new standalone convention. |
| !20600 | Merged master — deep/scanned | Bluetooth GATT registration stores UUID as dissector data and uses protocol-in-name-only registrations for hundreds of Decode-As-only entries, preserving UAT/CLI names while reducing registration cost. Strong architecture/performance example, but no distinct rule promoted beyond existing explicit-context and registration guidance. |
| !20599 | Open draft — down-weighted | Preference to preserve/clear custom window title remained draft/open with unresolved status. Not accepted precedent. |
| !20598 | Open — down-weighted | Expert Info background-color preference remained open with unresolved discussions. Not accepted precedent. |
| !20597 | Merged master — discussion-focused | 802.11 Block Ack presentation identifies the last correctly received frame rather than treating later zero bitmap entries as necessarily missed. Accepted protocol-semantics improvement; too protocol-specific for a new notebook rule. |
| !20596 | Merged master — deep/scanned | SCSI READ CAPACITY (10) now displays the raw zero-based last LBA and uses `last_lba + 1` only for derived block count/capacity. Strong corroboration of the existing rule separating wire-backed values from derived values. |
| !20595 | Merged release-4.2 — corroborating | Backport of Guy Harris's !20593 LIN dissector return-value fix. Strong stable-branch corroboration; master change carries the primary weight. |
| !20594 | Merged release-4.4 — corroborating | Backport of !20593. Same return-value semantics; no independent rule beyond the master fix. |
| !20593 | Merged master — deep, very high authority | Guy Harris-authored/merged LIN fix: when all bytes present in the tvbuff belong to the protocol, return `tvb_captured_length(tvb)`; bytes absent because of slicing cannot be claimed. Promoted to `dissector-return-value-conventions.md`. |
| !20592 | Merged master — scanned | Geneve spelling correction. No broader lesson. |
| !20591 | Merged master — scanned | Removes a NULL check that ran only after the pointer had already been dereferenced and inside a potentially large loop. Reinforces placing validation at the actual boundary rather than as ineffective repeated checks. |
| !20590 | Merged master — scanned | Decode-As handle descriptions moved to a hash table because uniqueness and lookup-by-description are core operations, improving TShark startup. Good data-structure/performance example; no new general convention. |
| !20589 | Merged master — scanned | Range registration looks up the dissector table once and adds Decode-As metadata once rather than repeating invariant work per integer. Reinforces registration/startup efficiency, without requiring a separate notebook rule. |

## Notebook changes from this run

- `dissector-return-value-conventions.md`: precise captured-byte/claimed-byte return semantics from !20593, corroborated by !20594/!20595.
- `registration-side-effect-conventions.md`: single logical registration / helper-side-effect rule from !20638, !20613, and !20615; structured compile-time OID representation guidance from !20604.
- `wiretap-interface-metadata-conventions.md`: preserve source-interface encapsulation and identity when creating output mappings, based on Guy Harris's resolved review in !20636.

No update to `reviewed-mrs.md` was required for correctness; this per-run ledger is authoritative for this batch and should be included when reconstructing the reviewed set for subsequent runs.
