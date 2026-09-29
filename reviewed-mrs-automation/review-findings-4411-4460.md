# Review findings: !4411–!4460

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged changes are primary evidence. Closed/unmerged work is explicitly down-weighted. Stable-branch backports are useful corroboration, but master-origin changes receive more architectural weight when the origin is available.

| MR | Outcome / weight | Finding |
|---|---|---|
| !4460 | merged, master | NGAP statistics via tap/stats-tree. Pascal Quantin caught an unrelated file edit and it was removed. |
| !4459 | merged, master | Display-filter range grammar accepts a general entity; semantic checking recursively proves fields/functions/nested ranges are sliceable. Positive and negative tests accompany the change. |
| !4458 | closed, low weight | Early MeshConnex/ExtremeMesh submission. Alexis La Goutte asked to split unrelated 802.11 work; Jaap Keuter flagged placement, value-string safety, ordering, and style. Not implementation precedent. |
| !4457 | merged, master | COSE leak fix uses a stack lookup key and packet-scope wmem ownership for a temporary Info-column copy instead of manual GLib allocation/free. |
| !4456 | merged, release-3.4 | CAM Inspector and VeriWave open-time heuristics stop after a bounded amount of data instead of scanning arbitrarily large candidate files; also avoids a large-file counter-overflow misdecision. |
| !4455 | merged, release-3.4 | Stable backport of !4421; corroborates the shared-UDP-conversation dispatch fix. |
| !4454 | merged, master | IDMP copies a packet-scoped protocol ID into longer-lived storage before retaining it. The code notes session ownership would be preferable. |
| !4453 | merged, master-3.2 | Release preparation; no new durable convention. |
| !4452 | merged, master | Adds NR-RRC registered entry points that set protocol/Info columns; useful context for !4437. |
| !4451 | merged, master | Fixes field registrations whose masks exceed registered width. Findings came from `check_typed_item_calls.py --mask`; fixes either widen the field/Boolean container or correct the mask. |
| !4450 | merged, release-3.4 | Release preparation; no new durable convention. |
| !4449 | merged, master | F1AP ASN.1 specification update/regeneration. |
| !4448 | merged, master | E1AP ASN.1 specification update/regeneration. |
| !4447 | merged, master | XnAP ASN.1 specification update/regeneration. |
| !4446 | merged, master | NRPPa ASN.1 specification update/regeneration. |
| !4445 | merged, master | NGAP ASN.1 specification update/regeneration. |
| !4444 | merged, release-3.4 | Debian symbol-file warning cleanup. |
| !4443 | merged, release-3.4 | CI diagnostic logging only. |
| !4442 | merged, master | `decode_bits_in_field()` gains explicit allocator scope; callers use `pinfo->pool` or the owning proto-tree scope instead of hidden ambient packet scope. |
| !4441 | merged, master | Reverts TCP analysis that misclassified the last out-of-order packet as a retransmission; transport-analysis changes need behavioral regression validation. |
| !4440 | merged, master | Continues explicit allocator-scope conversion for address-to-string helpers. |
| !4439 | merged, release-3.4 | Clang validation-script maintenance. |
| !4438 | merged, master | Adds an Ericsson eNode-B raw-log wiretap reader; no strong review discussion adding a project-wide rule. |
| !4437 | merged, master | Pascal Quantin rejected top-level column/root-protocol side effects in an ASN.1 decoder also used nested. Preferred pattern: named top-level wrapper/entry point around a context-neutral inner decoder. |
| !4436 | merged, master | X2AP ASN.1 specification update/regeneration. |
| !4435 | merged, master | S1AP ASN.1 specification update/regeneration. |
| !4434 | merged, release-3.4 | Stable backport of reload-error handling: `cf_reload()` returns status and Qt closes capture on failure. |
| !4433 | merged, release-3.4 | Stable backport of the reload-order fix represented on master by !4430. |
| !4432 | merged, master | Developer-guide markup cleanup. |
| !4431 | merged, master | Release-note update for Lua plugin reload behavior. |
| !4430 | merged, master | Lua reload calls `fieldsChanged()` before preference callbacks because callbacks may trigger reload/redissection and require rebuilt field state first. |
| !4429 | merged, master | Strengthens `check_typed_item_calls.py --mask`: non-zero mask bits outside registered field width are errors; redundant leading-zero digits are warnings. |
| !4428 | merged, release-3.4 | Stable Lua FileHandler reload backport. |
| !4427 | merged, master | wscbor gains indefinite-length string support and moves mutable internal chunk state behind a private structure for ABI stability. |
| !4426 | merged, master | Mechanical continuation of explicit allocator-scope threading for address-to-string helpers. |
| !4425 | merged, master | `proto_data` formatting uses `pinfo->pool` rather than ambient `wmem_packet_scope()`. |
| !4424 | merged, master | CBOR unit-test harness signal/jump handling. |
| !4423 | merged, master | Master-origin reload error handling: `cf_reload()` returns status; callers close capture when Lua FileHandler reload fails. |
| !4422 | merged, master | GRE bonding review caught one display-filter abbreviation registered repeatedly with incompatible FT types. Alexis La Goutte required type-specific abbreviations; John Thacker also corrected UTF-8 decoding. |
| !4421 | merged, master | BT-DHT and uTP can share one UDP conversation. BT-DHT re-validates each packet and returns 0 for non-DHT so the competing dissector can claim it. |
| !4420 | merged, release-3.4 | CI image-source migration to the project container registry. |
| !4419 | merged, master-3.2 | Automated registry/data update. |
| !4418 | merged, release-3.4 | Automated registry/data update. |
| !4417 | merged, master | Automated registry/translations/data update. |
| !4416 | merged, release-3.4 | IS-IS Prefix-SID stable fix; no broader review convention. |
| !4415 | merged, release-3.4 | Documentation POD cleanup. |
| !4414 | merged, release-3.4 | Explicit cast for a format warning in reassembly tests. |
| !4413 | merged, release-3.4 | JPEG/EXIF Copyright IFD tag correction. |
| !4412 | merged, release-3.4 | Obsolete minizip URL correction. |
| !4411 | merged, release-3.4 | Documentation POD cleanup. |

## Maintainer weighting

No substantive Guy Harris authored change or review comment appears in this 50-MR batch, so no Guy-derived position is inferred. Pascal Quantin's direct design review in !4437, John Thacker's merged master behavior in !4421, Martin Mathieson's checker work in !4429/!4451, and Alexis La Goutte's field-registration review in !4422 receive correspondingly strong weight.
