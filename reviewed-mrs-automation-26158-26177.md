# Supplemental Reviewed Wireshark Merge Requests — !26158–!26177

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`2261458afc3e287c111ffe13f534d9aaf5af07af`**
- Exactly twenty previously unreviewed MRs were reviewed: **!26158 through !26177 inclusive**.
- No MR outside that range was counted as reviewed in this batch.

## Weighting

Merged master-branch implementation and substantive human-review evidence received the greatest weight. Stable-branch cherry-picks/backports were recorded for ledger completeness and treated as corroboration rather than independent architectural evidence. Maintainer-authored or maintainer-directed changes from John Thacker, Gerald Combs, Anders Broman, Martin Mathieson, Pascal Quantin, and Alexis La Goutte were weighted strongly. No Guy Harris-authored or Guy Harris-reviewed MR appeared in this twenty-MR window, so no additional Guy-specific evidence was inferred.

| MR | State / target | Review depth | Durable finding or disposition |
|---|---|---|---|
| !26158 | merged, master | Shallow | WSUG clarification that `%APPDATA%` is the OS application-data directory and Wireshark uses its `Wireshark` child. Approved/merged by John Thacker. Documentation accuracy only; no new coding convention promoted. |
| !26159 | merged, release-4.6 | Shallow / backport | Backport of !26145. Replaces `tvb_new_subset_remaining()` with an explicitly length-bounded subset before handing a QUIC DATAGRAM payload to its child dissector. Strong corroboration of the already-recorded payload-boundary rule; not independent evidence. |
| !26160 | merged, master | Medium / documentation | Broad WSUG grammar/formatting cleanup marked AI-assisted, with human wording review from John Thacker before merge. Useful evidence that generated/editorial assistance remains subject to normal human review, but no distinct durable code convention promoted. |
| !26161 | merged, master | Medium | Adds IEEE 802.11 EHT PPE Thresholds dissection, approved/merged by Alexis La Goutte. Correct protocol feature work but little substantive review discussion; no general rule promoted. |
| !26162 | merged, master | Medium / maintainer-authored | Martin Mathieson adds/checks MAC-NR control elements relevant to NTN plus value-string/spec-table maintenance. Experienced-maintainer implementation, but primarily protocol-specific and not promoted as a new general convention. |
| !26163 | merged, master | Medium / maintainer-authored | Martin Mathieson adds missing MAC-NR downlink LCIDs/codepoints and associated generated/display coverage. Protocol-specific maintenance; no independent general convention promoted. |
| !26164 | merged, master | Deep / high authority review | AVRCP cover-art support with John Thacker review. John explicitly rejected a cross-dissector protocol-ID dependency and directed the contributor to resolve the OBEX protocol ID using `proto_get_id_by_filter_name()` in OBEX handoff. Strong corroboration of the notebook rule that protocol-specific lookup/dependency logic belongs in the consuming/owning dissector using existing registration APIs; existing `dissector-conventions.md` guidance was sufficient, so no duplicate section was added. |
| !26165 | merged, master | Deep / high authority | Procmon wiretap cleanup authored by Gerald Combs and approved/merged by John Thacker. Frees nested string allocations, centralizes private-state teardown, invokes cleanup on post-allocation open failure, registers `subtype_close` for successful opens, and clears `wth->priv` after teardown. Promoted to `memory-lifetime-conventions.md`. |
| !26166 | merged, master | Deep / high authority | John Thacker DICOM Export Objects overflow fix, merged by Anders Broman. Uses a size-appropriate representation and checked accumulation, stopping at the real `size_t` implementation limit rather than an arbitrary narrower maximum. Strong corroboration of existing checked-arithmetic/realistic-cap guidance; not duplicated. |
| !26167 | merged, master | Deep / high authority | John Thacker capture-session `GMutex` lifecycle fix. A mutex explicitly cleared at the end of a lifecycle must be initialized again before the next use, while failing to clear it can leak native resources on some platforms. Promoted synchronization init/clear pairing to `platform-gui-conventions.md`. |
| !26168 | merged, master | Deep / high authority review | IDN endless-loop fix. John Thacker explicitly corrected an equality-style loop condition and required continuing while the cursor/index remains below the endpoint. Promoted monotonic bounded-loop termination to `parser-edge-cases.md`. |
| !26169 | merged, master | Deep / highest platform authority | John Thacker removes the obsolete Windows `cli_main` compatibility layer after the supported-Windows baseline and application manifest guarantee UTF-8 active code pages; Gerald Combs approved/merged it. Additional tests exercise the guarantee. Added as implementation/testing follow-through for the notebook's platform-minimum policy rule in `platform-gui-conventions.md`. |
| !26170 | merged, master | Medium / maintainer-authored | Anders Broman adds GSM A BSSMAP Segmentation IE dissection. Accepted protocol implementation with no substantive review discussion; too protocol-specific for a new general rule. |
| !26171 | merged, master | Medium-high / high authority | John Thacker fixes an extcap failure-path leak reported by Coverity by freeing a buffer before the FIFO-open error return. Independently corroborates the centralized ownership/error-unwind rule promoted from !26165; no separate section added. |
| !26172 | merged, master | Deep / high authority | John Thacker SCTP reassembly hardening. Uses checked offset addition, clips fragment copies to allocated capacity, accumulates length in a wider type, caps to the implementation-supported size, and explicitly notes migration toward the standard Wireshark reassembly API. Very strong corroboration of existing reassembly/hostile-length arithmetic guidance in `dissector-conventions.md`; no duplicate rule added. |
| !26173 | merged, release-4.6 | Shallow / backport | Stable-branch cherry-pick of !26172 SCTP reassembly overflow fix. Corroboration only. |
| !26174 | merged, release-4.4 | Shallow / backport | Second stable-branch cherry-pick of !26172. Additional corroboration only. |
| !26175 | merged, release-4.6 | Shallow / backport | F1AP generated-dissector/configuration fix registering NotificationInformation in its extension role as well as its ordinary IE role. Stable-branch maintenance; no new general rule. |
| !26176 | merged, release-4.4 | Shallow / backport | Same F1AP fix as !26175 on release-4.4. Corroboration only. |
| !26177 | merged, master | Medium | IEEE 802.11 radio-duration correction, first contribution, approved/merged by Anders Broman. Downstream duration calculation now consumes the `fcs_len` already normalized by radiotap/configuration rather than a stale assumption. Useful architecture evidence for propagating normalized packet metadata, but this single straightforward instance was retained in the ledger rather than promoted to a new broad rule. |

## Notebook updates promoted from this batch

- `memory-lifetime-conventions.md`: owned wiretap/private parser state should have one teardown routine used by both post-acquisition failure paths and the registered close hook; nested allocations must be freed before their container and stale owner pointers cleared after final teardown (!26165, independently corroborated by !26171).
- `platform-gui-conventions.md`: synchronization primitives have matched initialize/clear lifecycles and reusable sessions must reinitialize after clear (!26167); once an explicitly agreed minimum-platform contract replaces a compatibility shim, remove the redundant shim and keep tests that exercise the external guarantee (!26169, building on !25993).
- `parser-edge-cases.md`: packet-walking loops should use monotonic range termination such as `cursor < end` and prove forward progress rather than rely on reaching an exact equality sentinel (!26168).

The QUIC payload-boundary backport (!26159), John Thacker's protocol-ID handoff review (!26164), DICOM checked-size work (!26166), and SCTP reassembly hardening plus stable backports (!26172–!26174) were intentionally retained as strong corroboration because equivalent durable guidance already exists in the notebook.