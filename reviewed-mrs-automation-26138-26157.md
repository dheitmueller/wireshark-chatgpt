# Supplemental Reviewed Wireshark Merge Requests — !26138–!26157

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`2261458afc3e287c111ffe13f534d9aaf5af07af`**
- Exactly twenty previously unreviewed MRs were reviewed: **!26138 through !26157 inclusive**.
- No MR outside that range was counted as reviewed in this batch.

## Weighting

Merged master-branch implementation and human review evidence received the greatest weight. Stable-branch cherry-picks/backports were recorded for ledger completeness and used as corroboration rather than independent architectural evidence. Maintainer-authored or maintainer-directed changes from Guy Harris, John Thacker, Gerald Combs, and Anders Broman were weighted particularly strongly.

| MR | State / target | Review depth | Durable finding or disposition |
|---|---|---|---|
| !26138 | merged, release-4.4 | Shallow | Gerald Combs release-note correction for security issue descriptions. Documentation/release maintenance only; no new durable coding convention promoted. |
| !26139 | merged, master | Deep / highest authority | Guy Harris O-RAN crash fix. A successful/available C-plane state object does not prove a separate lookup result exists; the exact `result` pointer must be checked before using `result->expected_sections`. Promoted independent-nullability/lookup guarding to `memory-lifetime-conventions.md`. |
| !26140 | merged, master | Deep | John Thacker IEEE 802.11 extended-tag correction, merged by Anders Broman. Accounts for the ordinary element ID and length bytes, presents the derived extended length as generated, and advances the cursor before validating remaining payload. Strong corroboration of accurate wire-byte accounting and cursor-relative bounds; existing notebook guidance is sufficient. |
| !26141 | merged, master | Deep / high authority | John Thacker Synchrophasor memory-hardening fix. Temporary packet-derived arrays are moved from file scope to `pinfo->pool` and copied into persistent arrays only after decoding, preventing persistent orphan allocations when exceptions interrupt parsing. Promoted to `memory-lifetime-conventions.md`. |
| !26142 | merged, release-4.6 | Shallow | Cherry-pick of the BLF FlexRay frame-flag fix previously reviewed as !26130. Corroboration only; not double-counted. |
| !26143 | merged, release-4.6 | Shallow | Cherry-pick of !26141. Strong stable-branch corroboration of the transient-allocation lifetime fix, but not independent evidence. |
| !26144 | merged, release-4.6 | Shallow | Guy Harris backport of the Touchlink redissection use-after-free fix previously reviewed in !26096. Corroboration only; existing redissection-lifetime notebook rule retained. |
| !26145 | merged, master | Deep | QUIC DATAGRAM payload-boundary fix, first contribution, merged by John Thacker. The code bounds the child dissector to the DATAGRAM payload rather than allowing succeeding QUIC frames into its tvb, reinforcing existing subset/boundary guidance. John also explicitly required both MR and commit subjects to put the dissector name before the colon rather than use generic `fix:`; promoted to `submission-conventions.md`. |
| !26146 | merged, master | Deep / high authority | John Thacker extcap Windows OVERLAPPED-I/O fix. The initial asynchronous read must be primed before attaching the GLib source; otherwise immediately available data can trigger the callback twice. Promoted to `extcap-runtime-conventions.md`. |
| !26147 | merged, master | Deep | New GSMTAP UM wiretap encapsulation and LTE MAC handoff, merged by Anders Broman. Includes a six-frame representative capture and automated tests covering encapsulation, RAR decoding, and the downstream RRC handoff chain. Strong corroboration of the notebook's existing representative-capture and end-to-end dissector-test expectations; no duplicate rule added. |
| !26148 | merged, master | Medium | Gerald Combs moves Wireshark release lifecycle material from the wiki into the User's Guide; merged by John Thacker. Documentation ownership/centralization improvement, but no distinct coding/review rule promoted. |
| !26149 | merged, master | Medium | Anders Broman F1AP ASN.1 configuration fix registers NotificationInformation in its extension role as well as its normal IE role; merged by John Thacker. Correct generated-dissector configuration work, but too protocol-specific for a new durable notebook rule. |
| !26150 | merged, master | Medium / high authority | Gerald Combs removes dead GTK-era `capture.columns` implementation while marking the preference obsolete; merged by John Thacker. Useful compatibility evidence that retired preferences should remain recognized as obsolete rather than simply vanish, but this batch alone was not used to create a broader preferences-policy rule. |
| !26151 | merged, master | Deep / high authority | John Thacker DICOM resource-exhaustion fix replaces repeated full-string rebuilding with `wmem_strbuf_t`; massif peak memory on the reproducer falls from roughly 5.3 GiB to 56.9 MiB. Promoted growable-string-buffer guidance to `memory-lifetime-conventions.md`. |
| !26152 | merged, master | Shallow | Gerald Combs User's Guide Asciidoctor cross-reference markup correction, merged by John Thacker. Documentation syntax only. |
| !26153 | merged, release-4.6 | Medium | Gerald Combs release-note platform-support wording update. Guy Harris later left an unresolved wording suggestion after merge. Useful editorial feedback but post-merge/unresolved and not a coding or architecture convention, so down-weighted. |
| !26154 | merged, release-4.6 | Shallow | Cherry-pick of !26151 DICOM `wmem_strbuf_t` fix. Stable-branch corroboration only. |
| !26155 | merged, release-4.4 | Shallow | Cherry-pick of !26151 DICOM `wmem_strbuf_t` fix. Second stable-branch corroboration only. |
| !26156 | merged, release-4.4 | Shallow | Backport of !26141 Synchrophasor transient-allocation fix. Stable-branch corroboration only. |
| !26157 | merged, master | Deep / high authority | John Thacker extcap teardown fix removes temporary directories belonging to control FIFOs as well as the FIFO files themselves. Promoted setup/teardown resource symmetry for auxiliary IPC channels to `extcap-runtime-conventions.md`. |

## Notebook updates promoted from this batch

- `memory-lifetime-conventions.md`: independently nullable lookup results must be checked before dereference (!26139); transient/speculative packet-derived arrays should remain packet-scoped until committed to persistent state (!26141, corroborated by !26143/!26156); repeated string appends should use `wmem_strbuf_t` rather than rebuilding full immutable strings (!26151, corroborated by !26154/!26155).
- `extcap-runtime-conventions.md`: prime Windows asynchronous/OVERLAPPED I/O before attaching the readiness source (!26146); teardown should remove auxiliary control-pipe filesystem resources as completely as setup created them (!26157).
- `submission-conventions.md`: dissector/component names belong before the colon in MR and commit subjects; generic action prefixes such as `fix:` do not substitute for the Wireshark component prefix (!26145).

MR !26147 was intentionally retained as corroboration rather than duplicated because `review-patterns.md` and `testing-fuzzing.md` already record the representative-capture and automated-dissector-test expectations it exemplifies.