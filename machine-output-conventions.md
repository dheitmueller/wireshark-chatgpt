# Wireshark Machine-Output Conventions

This file records durable conventions for machine-consumable output formats, where downstream schema and type semantics take precedence over human-oriented display formatting.

## Serialize according to the consumer's schema, not the field's display preference

A field's UI representation is not necessarily an appropriate wire representation for a structured export. Human-readable formatting can deliberately turn values into strings, add hexadecimal notation, or otherwise optimize presentation, while an ingestion-oriented format may promise concrete JSON or database types.

Merged MR !20871, authored by John Thacker, separates Elasticsearch/Kibana (`-T ek`) representation from generic display-oriented JSON representation. Integer fields that may legitimately be shown in hexadecimal are emitted as decimal integer values for EK because the generated Elasticsearch mapping declares them as integer types; booleans are emitted as JSON booleans. The change avoids asking Elasticsearch to infer or coerce display strings into the schema's numeric types.

**Implementation rule:** when an output mode targets a typed consumer, define serialization from that output contract independently of the packet-details presentation rules. Preserve numeric/boolean/string types required by the target schema even when the corresponding Wireshark field has a different preferred display base or textual rendering.

**Confidence:** Very high. Merged master output-correctness change authored by John Thacker, with the target schema/ingestion semantics stated explicitly in the MR rationale.

## Preserve multiplicity when flattening protocol trees into EK/JSON fields

Merged **!8802** fixes duplicate-field handling in TShark's EK output by collecting every occurrence under its registered field abbreviation and writing repeated instances through the EK writer. The change also updates DHCP EK golden baselines; output schemas must be validated with captures containing repeated fields, not only single instances.

**Review caution:** the patch changes ordered attribute-list traversal to `g_hash_table_foreach()`, whose iteration order is not an insertion-order contract. The merge is strong evidence for correct repeated-value grouping, but not a guarantee of stable serialized key order. When output determinism matters, add a separate ordered traversal and explicit golden-output tests. **Confidence:** high for the merged correctness fix, with ordering risk identified from its diff.

Merged **!8805** likewise documents that Elasticsearch's default string mapping need not be explicitly emitted by `elastic-mapping`; generated mappings should state the fields requiring non-default types rather than flooding the file with redundant string entries.
