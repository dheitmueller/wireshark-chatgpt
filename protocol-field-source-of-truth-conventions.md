# Wireshark Protocol Field Source-of-Truth Conventions

This file records how to resolve disagreements about the semantic type of packet fields.

## Register the semantic type defined by the protocol, not by an implementation anecdote

When deciding whether a field is text, bytes, an integer, or another semantic type, start from the specification that defines that exact field. An implementation's internal storage type, a neighboring field's behavior, or a library dictionary entry is secondary evidence and may describe a different contract.

Closed MR !127 proposed changing the TACACS+ Accounting Reply `data` field from `FT_STRING` to `FT_BYTES`. Guy Harris traced both the historical and then-current TACACS+ drafts. They describe this Accounting Reply field as text suitable for an administrative display, while separately stating that only fields specifically identified for protocol processing contain arbitrary bytes. Chuck Craft independently cited the same Accounting Reply language. The MR closed without the field-type change.

**Field rule:** choose the Wireshark field type from the normative semantics of the exact on-wire field. If an external implementation uses a generic octet container, verify whether that representation is merely an implementation choice before allowing it to override the protocol definition.

**Evidence rule:** a closed MR is not accepted implementation precedent. Here, however, Guy Harris's standards analysis is durable review evidence because it explains why the proposed type change was wrong.

**Confidence:** Very high for the source-of-truth rule; low for the rejected implementation, which must not be copied.
