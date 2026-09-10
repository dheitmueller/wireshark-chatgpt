# Automated Wireshark MR Review: !24759-!24808

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from all files in `reviewed-mrs-automation/`, including the aggregate `reviewed-mrs-automation.md`, plus `reviewed-mrs.md`. The historical !17571-!17620 batch is preserved and counted. The aggregate ledger confirmed that !25788-!25827 had already been reviewed despite the gap in the per-run filename sequence. The 50 highest-numbered corpus MRs not present in the resulting reviewed set were therefore !24808 down through !24759, inclusive.

Exactly 50 MRs were reviewed in this run. Merged MRs and substantive maintainer review were weighted most heavily; open, draft, closed, superseded, and mistaken-target MRs were retained as lower-weight evidence.

| MR | State / weight | Review result |
|---|---|---|
| !24808 | merged | Sharkd security-warning documentation; useful security context, no new coding convention. |
| !24807 | merged/backport | FetchArtifacts ordering/update maintenance; no durable new convention. |
| !24806 | merged | FetchArtifacts Windows dependency update/order cleanup; no durable new convention. |
| !24805 | closed draft, down-weighted | V2GTP dissector experiment. Martin Mathieson recommended ordinary string column APIs instead of format APIs when no formatting is needed; not promoted because the MR was abandoned. |
| !24804 | merged | BACApp text correction; no durable coding convention. |
| !24803 | merged | Restored newer libgcrypt after the Windows library-lifetime problem behind !24787 was repaired; corroborates platform handle-domain guidance. |
| !24802 | merged | John Thacker removed temporary string copies by using packet-buffer-to-integer helpers directly; reinforces existing fetch/convert-once guidance. |
| !24801 | merged | Lua debugger break-on-error support; no broadly reusable convention extracted. |
| !24800 | merged | Geneve GCP option support included an example capture/expected behavior; reinforces capture-backed protocol testing. |
| !24799 | merged | O-RAN FH CUS all-zero beamforming bundle handling; protocol-specific validation, no new general rule. |
| !24798 | closed/superseded, down-weighted | Earlier Geneve GCP attempt superseded by merged !24800. |
| !24797 | open, down-weighted | Relative-capture-time fallback proposal; not treated as accepted architecture. |
| !24796 | open draft, down-weighted | pcapng Name Resolution block numbering work; not treated as accepted architecture. |
| !24795 | merged, deep review | New Livewire dissector with extensive Jaap Keuter/Martin Mathieson review. Reinforces config.h-first, meaningful blurbs, correct field display bases such as `BASE_PT_UDP`, precise byte ranges, and avoiding unnecessary formatting APIs. Most guidance was already represented in the notebook. |
| !24794 | merged | John Thacker added signed counted-buffer-to-integer helpers; reinforces parsing packet text without unnecessary NUL-terminated copies. |
| !24793 | merged | John Thacker set TLS apphandle explicitly for HTTP-over-TLS tunneling instead of relying on generic conversation existence; reinforces explicit context propagation. |
| !24792 | closed draft, down-weighted | Earlier libgcrypt revert/retry work; superseded by merged !24803. |
| !24791 | merged | John Thacker fixed NTS-KE ownership after an exporter returned a newly allocated buffer; reinforces allocator/lifetime conventions. |
| !24790 | merged | John Thacker converted more dissectors to direct counted-string integer APIs; reinforces fetch/convert-once guidance. |
| !24789 | merged | Martin Mathieson eliminated more double packet fetches; reinforces existing `_ret_`/reuse guidance. |
| !24788 | merged, high authority | Guy Harris clarified `WS_NONSTRING`: it means not NUL-terminated, not non-character data. Authoritative but intentionally kept as narrow semantic documentation evidence. |
| !24787 | merged, deep/high authority | Windows libgcrypt startup regression. Guy Harris explicitly warned against treating `HMODULE`, `HANDLE`, and `void *` as interchangeable merely because representations may match. Promoted as platform handle-domain guidance; !24803 corroborates the eventual fix. |
| !24786 | merged | Continued ISAKMP-to-IKE source-file rename; reinforces staged compatibility-preserving terminology migrations. |
| !24785 | merged | John Thacker fixed ROHC large-CID off-by-one that could write through NULL; reinforces exact remaining-length accounting. |
| !24784 | merged | John Thacker fixed Coverity integer-overflow findings; reinforces arithmetic-safety guidance. |
| !24783 | merged, high weight | John Thacker distinguished GitLab MR source SHA from the synthetic merge-result SHA. Promoted as CI source-identity guidance. |
| !24782 | merged | John Thacker added signed counted-buffer conversion helpers plus tests; reinforces shared parser APIs. |
| !24781 | merged/backport | rclone CI backport to release-4.4; no new durable rule. |
| !24780 | merged/backport | rclone CI backport to release-4.6; no new durable rule. |
| !24779 | closed, down-weighted | OpenFlow nested-length/infinite-loop fix description favors tvb subsets and containing-structure boundaries, but this MR was closed and therefore retained only as lower-weight corroboration. |
| !24778 | merged | rclone CI follow-up fix; no separate durable rule. |
| !24777 | merged | CI typo fix; no durable rule. |
| !24776 | merged | John Thacker migrated ISAKMP naming toward IKE while retaining `isakmp` as an alias. Strong example of standards-correct naming with compatibility preservation; existing field/compatibility guidance was sufficient. |
| !24775 | merged | Qt endpoint/conversation statistics correction after display-filter toggling; no broad new rule. |
| !24774 | merged | Initial rclone CI transition; infrastructure-specific, no separate rule promoted. |
| !24773 | merged | Bluetooth HCI enumeration text completeness; no broad new convention. |
| !24772 | merged, high weight | John Thacker fixed kNet varint length decoding so the wire encoding itself constrains the value enough to make later signed overflow impossible. Strong corroboration for arithmetic bounds derived from the encoding domain. |
| !24771 | merged | GTP offsets changed to unsigned; reinforces semantic-domain integer typing. |
| !24770 | merged | Lua debugger documentation shortcuts; no code convention. |
| !24769 | merged | John Thacker used `tvb_memeql` rather than copying packet bytes to a temporary buffer and initialized a Coverity-flagged buffer; reinforces direct tvbuff operations/initialization. |
| !24768 | merged | John Thacker fixed `sizeof(char *)` being mistaken for dynamically allocated buffer size; reinforces ownership/size provenance. |
| !24767 | merged, high weight | Removed dumpcap `TCP@` support specifically to reduce attack surface in an often-elevated process when unprivileged tools can provide the same composition. Promoted as privilege-boundary guidance. |
| !24766 | merged | John Thacker used `tvb_get_string_uint` directly and noted that separate conversion can avoid copies when a tree item is faked; reinforces existing packet-buffer conversion guidance. |
| !24765 | merged | Moved compatibility constant to `glib-compat.h`; no separate broad rule. |
| !24764 | merged | Lossless resource PNG compression; no durable engineering convention needed. |
| !24763 | merged/backport | RTPS-VT struct initialization release-4.4 backport; corroborates master fix !24759. |
| !24762 | merged/backport | RTPS-VT struct initialization release-4.6 backport; corroborates master fix !24759. |
| !24761 | merged | O-RAN validation for prohibited `symInc`; reinforces protocol-semantic validation. |
| !24760 | merged, deep review | DECT NR CVG addition with 97 review notes. Stig Bjørlykke requested a sample capture and directed undecoded content to be represented by a real bytes item with expert info attached to that item; he also corrected an over-eager idea of expert-marking every reserved value. Promoted the precise undecoded-data diagnostic pattern. |
| !24759 | merged, high weight | John Thacker initialized RTPS-VT context before partial parsing so missing parameters cannot propagate uninitialized state; reinforced by stable backports !24762/!24763 and existing initialization guidance. |

## Durable notebook updates from this batch

- `ci-source-identity-conventions.md`: distinguish an MR source commit from GitLab's synthetic merge-result commit when source identity matters (!24783).
- `platform-handle-domain-conventions.md`: preserve platform-defined opaque handle domains and their matching lifetime APIs; do not erase/interchange `HMODULE`, `HANDLE`, and `void *` based on representation alone (Guy Harris review in !24787, corroborated by !24803).
- `privilege-boundary-conventions.md`: minimize functionality and attack surface inside elevated capture helpers when an unprivileged composition can provide the same feature (!24767).
- `undecoded-data-diagnostic-conventions.md`: expose undecoded bytes as an actual field and attach expert information to that precise item/range; do not expert-mark reserved values reflexively (!24760).

`reviewed-mrs.md` was not changed; this exact per-run ledger is the authoritative audit record for this batch.
