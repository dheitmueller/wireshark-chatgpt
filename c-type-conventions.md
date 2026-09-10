# Wireshark C Type Conventions

This file records durable type-usage conventions extracted from accepted Wireshark review. Current upstream source and coding guidance remain authoritative.

## Prefer standard C fixed-width integer types in Wireshark-owned dissector code

Wireshark-owned code should not introduce GLib integer aliases where standard C integer types express the value domain directly. Use types such as `uint8_t` for protocol-width values unless an external API contract requires a library-defined type.

In merged MR !24689, Jaap Keuter reviewed new ENIP dissector code and explicitly requested replacing the newly introduced GLib types with `uint8_t` throughout the change, noting that Wireshark was no longer using those GLib types. Michael Mann pointed the contributor to `tools/convert-glib-types.py`; the contributor applied the review, Anders Broman approved, and the MR merged.

**Implementation rule:** for Wireshark-internal protocol values, prefer the project's standard C/fixed-width types rather than legacy GLib scalar aliases. Preserve dependency-defined scalar types at true external API boundaries when the called API's signature requires them; do not mechanically convert across an ABI/API contract.

**Confidence:** Very high for new Wireshark-owned dissector code. Direct Jaap Keuter review on a merged MR, consistent with the existing notebook distinction that external dependency API types must be preserved at their boundaries.
