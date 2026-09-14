# Automated MR review ledger: !20489–!20538

Corpus revision reviewed: `dheitmueller/wireshark-corpus-mrs@9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all available per-run files under `reviewed-mrs-automation/`; preserved and counted the historical !17571–!17620 batch. Existing per-run tracking covers the descending corpus through !20539. The corpus was checked by actual MR objects/prefix searches rather than assuming that numeric ranges were complete. Every MR object !20489 through !20538 exists at the pinned corpus revision, and none belongs to the previously reviewed set. This run therefore reviewed exactly those 50 MRs, newest to oldest.

Weighting: merged master changes and substantive maintainer discussion receive the most weight; stable/release maintenance corroborates corresponding master behavior; open, closed, abandoned, or superseded work is recorded but not treated as accepted precedent. Guy Harris technical feedback is treated as especially authoritative.

| MR | Review depth / disposition | Notes |
|---:|---|---|
| !20538 | Merged master — scanned | Automatic registry/manufacturer/service-number update. Generated data maintenance; no new convention. |
| !20537 | Merged release-4.2 — scanned | Automatic data update corresponding to the master maintenance batch. |
| !20536 | Merged release-4.4 — scanned | Automatic data update corresponding to the master maintenance batch. |
| !20535 | Merged master — discussion-focused | Disambiguates an expert-info filter name from an hf filter name after epan warned about the same filter identifier having incompatible types. Useful registration-namespace evidence; no standalone rule added. |
| !20534 | Merged master — deep, Michael Mann | Restores early `console.log.level` handling in `ws_log` because logging can occur before normal preference processing. Promoted to `initialization-lifecycle-conventions.md`. |
| !20533 | Merged master — deep, John Thacker | Centralizes IDB normalization so writers do not advertise timestamp precision finer than `nstime_t` can preserve. Promoted to `wiretap-interface-metadata-conventions.md`. |
| !20532 | Merged master — scanned | Moves X.264/ISO transport IDs from a public epan header into the sole X.25 dissector consumer. Corroborates source-locality/layering guidance. |
| !20531 | Merged master — discussion-focused | Defines user-oriented display-filter equality semantics for NaN instead of inheriting broken accidental behavior; John Thacker reviewed edge cases. Domain-specific semantic choice, not promoted as a general IEEE rule. |
| !20530 | Merged master — deep, Michael Mann | Resolves stable protocol IDs once in handoff rather than repeatedly calling `proto_get_id_by_filter_name()` in packet dissection. Promoted to `initialization-lifecycle-conventions.md`. |
| !20529 | Merged master — scanned | Adds xxHash and uses it for strong `wmem_map` hashing. Performance implementation; no additional convention. |
| !20528 | Merged master — corroborating | Broad cleanup of ambient `wmem_packet_scope()` usage. Reinforces explicit allocator-context rules already recorded. |
| !20527 | Merged master — discussion-focused, Guy Harris review | Adds ZigBee Touchlink key-exchange decryption. Guy questioned creating commissioning state for commands outside the commissioning command set; useful state-boundary review evidence, but unresolved discussion was not promoted as settled precedent. |
| !20526 | Merged master — scanned | Stores dependency-graph adjacency as a set/hash table to avoid duplicate/linear behavior. Internal data-structure optimization. |
| !20525 | Merged master — deep, Guy Harris review | Adds first BLF writing support. Guy explicitly preferred checking `wtap_dump_file_seek()`'s Boolean return over calling it and then inspecting the error out-parameter; corroborates existing API-status contract guidance. |
| !20524 | Merged master — scanned | Lua console paste path requests plaintext so IDE clipboard formatting is not injected. UI input handling; no broad rule extracted. |
| !20523 | Merged master — scanned | Corrects UDS RDBPI response payload interpretation to match the standard. Protocol-specific correctness. |
| !20522 | Merged master — corroborating | Converts `val64_to_str` API to require an explicit wmem scope, removing ambient packet-scope allocation. Reinforces allocator-scope convention. |
| !20521 | Merged master — scanned | Adds Strato, the command-line Stratoshark frontend adapted from TShark. Major feature but little durable review guidance in the corpus discussion. |
| !20520 | Merged master — scanned | Adds ILNP addressing support to DCCP. Protocol-specific extension. |
| !20519 | Merged master — corroborating | Removes `char_val_to_str()` to eliminate remaining ambient packet-scope allocation. Reinforces allocator-scope guidance. |
| !20518 | Merged master — scanned | SMB null-check correctness fix. Narrow defensive bug fix. |
| !20517 | Merged master — scanned | Moves `gpa_name_map` to `wmem_map` so capacity can be reserved. Performance/capacity work already represented by lookup-table guidance. |
| !20516 | Merged master — scanned | Separates generated Bluetooth data into its own pregenerated source file to simplify updates. Corroborates generated-source reproducibility/ownership guidance. |
| !20515 | Merged master — corroborating | Makes string-to-string helpers require explicit allocator scope and removes an ambient-scope variant. Reinforces allocator API rules. |
| !20514 | Merged master — scanned | Avoids traversing heuristic dissector GSLists twice. Straightforward hot-path performance cleanup. |
| !20513 | Merged master — scanned | Avoids double hashing when checking dissector-table/name uniqueness. Internal registration performance cleanup. |
| !20512 | Merged master — scanned | Adapts bundled QCustomPlot code to Qt 6.9 `QImage::flipped` API. Dependency compatibility maintenance. |
| !20511 | Merged master — scanned | Uses hash-table insertion semantics to avoid a second hash lookup when detecting duplicate protocol names. Internal performance cleanup. |
| !20510 | Merged master — corroborating | Kerberos string members become allocator-owned pointers with lifetime matching their containing structures. Reinforces explicit ownership/lifetime rules. |
| !20509 | Merged master — scanned | Frees KNX/IP keyring XML documents on all completed/error paths. Resource-lifetime correctness. |
| !20508 | Merged master — scanned | Fixes WiMAX ASN CP dictionary leaks by freeing regex results and GSList members appropriately. Resource-lifetime maintenance. |
| !20507 | Merged master — corroborating | Adds `wmem_map` minimum-capacity reservation to avoid costly rehash/copy growth. Already represented by immutable lookup/capacity guidance. |
| !20506 | Merged master — scanned | Improves etwdump extcap usability and option presentation. Feature/UI work with no new project-wide rule extracted. |
| !20505 | Closed — down-weighted | Documentation section-number adjustment for display-filter deprecation text; closed and not treated as accepted precedent. |
| !20504 | Merged master — corroborating | Makes bytes/range string-format APIs take explicit memory scopes. Reinforces explicit allocator-context conventions. |
| !20503 | Merged master — scanned | Adds a size/overflow guard identified by static analysis. Corroborates arithmetic-safety guidance. |
| !20502 | Merged master — scanned | Fixes PEAK CAN TRC double-free/use-after-free reported by Coverity. Memory-safety maintenance. |
| !20501 | Merged master — scanned | Clarifies O-RAN uplink preferences with a usage hint. Preference UX/documentation improvement. |
| !20500 | Merged master — deep, John Thacker | Compares pcapng `if_tsresol` using its specified default: absent and explicit 6 are semantically equivalent, while absent and non-6 differ. Promoted to `wiretap-interface-metadata-conventions.md`. |
| !20499 | Merged stable/release fix — corroborating | IPsec padding all-zeroes control-flow correction corresponding to the master fix. |
| !20498 | Merged master — scanned | Adds an officially assigned MCC/MNC entry from ITU data. Registry-data maintenance. |
| !20497 | Merged master — scanned | Makes `check_filter_matches.py` accept profile/frame-limit arguments for practical validation runs. Testing-tool enhancement; no new rule. |
| !20496 | Merged master — scanned | Fixes nested-loop control flow in IPsec all-zero padding validation. Narrow parser correctness fix. |
| !20495 | Merged master — deep, John Thacker | Defers very large X11/ASTERIX/ERF field registration behind controlled dissector entry points, with measured >10% empty-capture TShark startup improvement. Added as corroborating evidence to existing lazy-registration guidance. |
| !20494 | Merged master — scanned | Removes obsolete `HAVE_HFI_SECTION_INIT` build conditional. Cleanup only. |
| !20493 | Merged master — scanned | Moves `dot11decrypt_ctx` definition into the crypt component where it is declared to improve epan linkage boundaries. Corrobrates locality/layering. |
| !20492 | Closed/superseded — down-weighted | Earlier ZigBee Touchlink decryption MR superseded by accepted !20527; not treated as precedent. |
| !20491 | Merged master — corroborating | Replaces direct `printf()` diagnostics in Wiretap readers with `ws_log`. Reinforces existing logging-framework guidance. |
| !20490 | Open/unmerged — down-weighted, Guy Harris comment | Packet-comment keyboard shortcut change. Guy noted the MR contained unrelated changes and only the dialog file belonged; useful focused-MR evidence but not accepted precedent at this snapshot. |
| !20489 | Closed/superseded — down-weighted | Attempted Qt date/time deprecation fix changed local/UTC behavior; John Thacker identified the semantic regression and pointed to !20480 as another solution. Not promoted. |

## Durable notebook changes from this run

- `wiretap-interface-metadata-conventions.md`: writers must not advertise timestamp precision the in-memory representation cannot preserve, and representation-limited metadata normalization belongs at the common dump boundary (!20533).
- `wiretap-interface-metadata-conventions.md`: optional metadata used for identity/equality must be compared using specified default semantics; absent `if_tsresol` is equivalent to explicit value 6, not to an arbitrary value (!20500).
- `initialization-lifecycle-conventions.md`: stable protocol registry identities should be resolved once during handoff rather than repeatedly in packet dissection (!20530).
- `initialization-lifecycle-conventions.md`: subsystem controls needed before normal preferences are available must be parsed at the earliest phase required by first use; early logging control belongs in `ws_log` argument handling (!20534).
- `initialization-lifecycle-conventions.md`: merged !20495 is added as measured corroboration for the existing rule that lazy registration is appropriate only behind controlled entry points while preserving the observable registration contract.

No update to `reviewed-mrs.md` was required; this exact per-run ledger is authoritative for this batch.