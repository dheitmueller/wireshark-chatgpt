# Wireshark Decode As Conventions

This file records durable conventions for dissector registration metadata that is exposed through Decode As. Current upstream source remains authoritative.

## Give every Decode-As-visible dissector handle a unique human description

A dissector table used by Decode As exposes registered handle descriptions in the UI. Multiple handles that belong to the same protocol can therefore not rely on the protocol's shared description if users need to distinguish the choices.

Merged master MR !20567, authored by John Thacker and merged by Anders Broman, changes Git and EDHOC media-type registrations to use `create_dissector_handle_with_name_and_description()` with distinct descriptions such as advertisement/request/result and EDHOC-with-CID variants. Its rationale explicitly says that the media-type table is used for Decode As and that descriptions therefore need to be unique. Companion merged MR !20562 applies the same cleanup to CMP, OCSP, and PKIXTSP variants; together these changes made descriptions unique across Decode-As tables, including string-keyed tables.

**Implementation rule:** when more than one dissector handle for a protocol is registered in a table that participates in Decode As, give each choice a stable unique handle name/description that conveys the variant the user is selecting. Do not assume the common protocol long/short name is sufficient metadata for all handles.

**Review implication:** inspect Decode As presentation when adding multiple table entries that share a protocol ID, especially media-type/string tables. Registration metadata is user-facing behavior, not merely an internal label.

**Confidence:** Extremely high. Merged master work authored by John Thacker, accepted by Anders Broman, and applied across multiple unrelated dissectors.

## Use the actual dissector-table match key when direction logic must survive Decode As

A dissector bound through a transport table can be selected on a user-chosen Decode As port rather than only on its conventional/IANA port. Direction logic that compares the packet ports against a hard-coded service port therefore breaks when the same dissector is rebound.

During review of MR !9658, John Thacker explicitly requested using `pinfo->match_uint` to determine which TCP port selected the dissector, comparing that dispatch-table key with `pinfo->destport` or `pinfo->srcport`. He noted that this keeps the code correct when the dissector is assigned to another port through Decode As. !9658 was later superseded for branch-history reasons by merged MR !10124, and the accepted MS-DO dissector retains the `match_uint` direction pattern.

**Implementation rule:** when a transport-table dissector needs to know which endpoint corresponds to the registered/selected service, use the table match value supplied in `packet_info` rather than assuming the protocol's default port. Treat the conventional port as registration metadata, not as the authoritative identity of the dispatch that actually occurred.

**Confidence:** Very high. Direct John Thacker review, followed by a merged successor implementation and continued upstream use of the requested pattern.

## Preserve parent context through Decode As dispatch

Merged master MR !9071 adds GRE payload Decode As support. In review, Alexis La Goutte asked how to preserve `gre_hdr_info`; John Thacker pointed to `dissector_try_payload_new()`, which accepts the parent data pointer, and the merged implementation uses it. Decode As should therefore be treated as another route into the same dissector call contract rather than a reason to drop required parent context. John also suggested checking whether an existing keyed table can expose Decode As directly before adding a parallel fallback table.
