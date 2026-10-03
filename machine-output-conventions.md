# Wireshark Machine-Output Conventions

This file records durable conventions for machine-consumable output formats, where downstream schema and type semantics take precedence over human-oriented display formatting.

## Serialize according to the consumer's schema, not the field's display preference

A field's UI representation is not necessarily an appropriate wire representation for a structured export. Human-readable formatting can deliberately turn values into strings, add hexadecimal notation, or otherwise optimize presentation, while an ingestion-oriented format may promise concrete JSON or database types.

Merged MR !20871, authored by John Thacker, separates Elasticsearch/Kibana (`-T ek`) representation from generic display-oriented JSON representation. Integer fields that may legitimately be shown in hexadecimal are emitted as decimal integer values for EK because the generated Elasticsearch mapping declares them as integer types; booleans are emitted as JSON booleans. The change avoids asking Elasticsearch to infer or coerce display strings into the schema's numeric types.

**Implementation rule:** when an output mode targets a typed consumer, define serialization from that output contract independently of the packet-details presentation rules. Preserve numeric/boolean/string types required by the target schema even when the corresponding Wireshark field has a different preferred display base or textual rendering.

**Confidence:** Very high. Merged master output-correctness change authored by John Thacker, with the target schema/ingestion semantics stated explicitly in the MR rationale.
## Treat deliberate structured-output schema changes as migrations

A machine-readable output format is an interface even when the project does not promise indefinite wire compatibility. When a development-branch change intentionally replaces that schema, the change should make the new structure unambiguous and give consumers enough information to migrate.

Merged master MR !184 replaces the Follow Stream YAML layout with explicit `peers` and `packets` sections carrying peer identity, per-peer index, timestamp, and data. During review, Jan Novak explicitly accepted changing the format on master but required the old format to be replaced rather than ambiguously coexisting, a release-note entry, user-guide documentation of the new schema, and an explanation mapping old keys such as `peer0_2` onto the new peer/index representation. Those requests were incorporated before merge.

**Output rule:** if a structured CLI/UI export schema is intentionally changed, update the producer and all public documentation together. Record the compatibility break in release notes and, where practical, document how old fields/keys map to the replacement schema.

**Review rule:** test/document the output as a consumer-visible data contract, not merely as pretty text. A schema change that is acceptable on a development branch can still impose real migration work on scripts.

**Confidence:** Very high for the schema/documentation rule. Direct maintainer review was incorporated into the merged master MR.
