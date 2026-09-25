# Wireshark Machine-Output Conventions

This file records durable conventions for machine-consumable output formats, where downstream schema and type semantics take precedence over human-oriented display formatting.

## Serialize according to the consumer's schema, not the field's display preference

A field's UI representation is not necessarily an appropriate wire representation for a structured export. Human-readable formatting can deliberately turn values into strings, add hexadecimal notation, or otherwise optimize presentation, while an ingestion-oriented format may promise concrete JSON or database types.

Merged MR !20871, authored by John Thacker, separates Elasticsearch/Kibana (`-T ek`) representation from generic display-oriented JSON representation. Integer fields that may legitimately be shown in hexadecimal are emitted as decimal integer values for EK because the generated Elasticsearch mapping declares them as integer types; booleans are emitted as JSON booleans. The change avoids asking Elasticsearch to infer or coerce display strings into the schema's numeric types.

**Implementation rule:** when an output mode targets a typed consumer, define serialization from that output contract independently of the packet-details presentation rules. Preserve numeric/boolean/string types required by the target schema even when the corresponding Wireshark field has a different preferred display base or textual rendering.

**Confidence:** Very high. Merged master output-correctness change authored by John Thacker, with the target schema/ingestion semantics stated explicitly in the MR rationale.

## Keep machine-facing identifiers self-consistent across producer and consumer endpoints

A value emitted by one machine-facing API can become input to another API. In that case its textual representation is part of the interoperability contract, even when several representations would describe the same numeric value to a human.

Merged master MR !9919 fixes Sharkd RTP APIs where the rtp-streams tap emitted SSRC as an unsigned decimal value while rtp-analyse and rtp-download tokens expected a hexadecimal string parsed by ws_hexstrtou32(). The accepted change emits the SSRC consistently as a hexadecimal string and adds a representative capture plus self-tests covering the producer and consumer paths. Gilbert Ramirez also recommended documenting Sharkd's special error codes in repository documentation, noting that the documentation work could follow separately.

**API rule:** when an output field is intended to be copied into another command, token, or endpoint, test the round trip and keep representation, type, and accepted syntax aligned across both sides. Treat that representation as a machine API contract, not merely display formatting.

**Documentation rule:** machine-facing status/error codes and token formats should be documented even when the code change itself is small enough to merge first.

**Confidence:** High. Merged master API consistency fix with focused self-tests and explicit maintainer documentation feedback.
