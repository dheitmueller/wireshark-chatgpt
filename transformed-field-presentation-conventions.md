# Transformed Field Presentation Conventions

This file records durable Wireshark conventions for protocol-tree fields whose displayed semantic value is obtained only after transforming protected, encoded, or otherwise non-literal packet bytes. Current upstream source remains authoritative.

## Mark transformed values generated when the displayed value is not literally present at the cited TVB bytes

A protocol-tree item can point at bytes that are the source of a value without those bytes literally containing the value shown to the user. If the dissector first decrypts, unmasks, deprotects, combines, or otherwise transforms the source before producing the displayed value, presenting the result as an ordinary wire-backed item can mislead users about what appears in the capture.

Merged master MR !11079, authored and merged by John Thacker, fixes QUIC fields affected by header protection. The dissector already had the deprotected values for the reserved bits, packet-number length, key phase, and packet number, but the tree items were associated with packet offsets containing the protected representation. The accepted change keeps those useful decoded fields and marks each item with `proto_item_set_generated()`, explicitly to reduce confusion. The MR notes that creating a separate data source for this transformation would be excessive for this case.

**Presentation rule:** when the value passed to `proto_tree_add_*()` is not the literal value represented by the referenced TVB bytes because an earlier transformation changed it, mark the item generated unless the transformed byte stream is itself exposed as a separate data source and the item is attached to that transformed source.

**Review rule:** distinguish interpretation from transformation. A wire value does not become generated merely because the dissector maps an enum, applies a field mask/shift through the normal field machinery, or gives the bytes a useful name. Generated status is appropriate when the displayed value depends on state or processing that makes it differ from what those packet bytes directly encode.

**Data-source rule:** use an additional data source when users or downstream dissectors benefit from inspecting the transformed byte sequence as a coherent buffer. For a small set of derived/deprotected scalar fields where a separate buffer would add little value, generated items can communicate the distinction more simply.

Merged !11089 provides corroborating evidence from a different transformation domain: PFCP's Quota Validity Time carries a relative number of seconds, while the dissector also computes and displays a human-readable absolute timestamp from the packet time plus that value; the accepted implementation marks the derived timestamp generated.

**Confidence:** Very high. Merged master field-presentation fix authored and merged by John Thacker, independently corroborated by another merged derived-value presentation change.
