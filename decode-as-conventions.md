# Wireshark Decode As Conventions

This file records durable conventions for dissector registration metadata that is exposed through Decode As. Current upstream source remains authoritative.

## Give every Decode-As-visible dissector handle a unique human description

A dissector table used by Decode As exposes registered handle descriptions in the UI. Multiple handles that belong to the same protocol can therefore not rely on the protocol's shared description if users need to distinguish the choices.

Merged master MR !20567, authored by John Thacker and merged by Anders Broman, changes Git and EDHOC media-type registrations to use `create_dissector_handle_with_name_and_description()` with distinct descriptions such as advertisement/request/result and EDHOC-with-CID variants. Its rationale explicitly says that the media-type table is used for Decode As and that descriptions therefore need to be unique. Companion merged MR !20562 applies the same cleanup to CMP, OCSP, and PKIXTSP variants; together these changes made descriptions unique across Decode-As tables, including string-keyed tables.

**Implementation rule:** when more than one dissector handle for a protocol is registered in a table that participates in Decode As, give each choice a stable unique handle name/description that conveys the variant the user is selecting. Do not assume the common protocol long/short name is sufficient metadata for all handles.

**Review implication:** inspect Decode As presentation when adding multiple table entries that share a protocol ID, especially media-type/string tables. Registration metadata is user-facing behavior, not merely an internal label.

**Confidence:** Extremely high. Merged master work authored by John Thacker, accepted by Anders Broman, and applied across multiple unrelated dissectors.