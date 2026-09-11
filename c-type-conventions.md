# Wireshark C Type Conventions

This file records durable type-usage conventions extracted from accepted Wireshark review. Current upstream source and coding guidance remain authoritative.

## Prefer standard C fixed-width integer types in Wireshark-owned dissector code

Wireshark-owned code should not introduce GLib integer aliases where standard C integer types express the value domain directly. Use types such as `uint8_t` for protocol-width values unless an external API contract requires a library-defined type.

In merged MR !24689, Jaap Keuter reviewed new ENIP dissector code and explicitly requested replacing the newly introduced GLib types with `uint8_t` throughout the change, noting that Wireshark was no longer using those GLib types. Michael Mann pointed the contributor to `tools/convert-glib-types.py`; the contributor applied the review, Anders Broman approved, and the MR merged.

**Implementation rule:** for Wireshark-internal protocol values, prefer the project's standard C/fixed-width types rather than legacy GLib scalar aliases. Preserve dependency-defined scalar types at true external API boundaries when the called API's signature requires them; do not mechanically convert across an ABI/API contract.

**Confidence:** Very high for new Wireshark-owned dissector code. Direct Jaap Keuter review on a merged MR, consistent with the existing notebook distinction that external dependency API types must be preserved at their boundaries.

## Do not assume the underlying C type of typedefs such as `time_t` when formatting

A typedef's concrete integer type can vary by platform. Format it through a representation whose format contract is known, or use an existing Wireshark formatting helper, rather than assuming that the typedef maps to `long`, `int`, or another particular base type.

Merged MR !23925, authored and merged by John Thacker, fixes TRDP formatting that used `%ld` for `nstime.secs`: `time_t` can be `long long` on supported platforms such as MSYS2. The accepted fix casts to `intmax_t` and uses `%ji`; John also noted that the existing `display_signed_time()` helper could be appropriate for this kind of display.

**Implementation rule:** when formatting platform-dependent typedefs, either convert to a known-width/max-width C type and use its matching format specifier or use the project's semantic formatting helper. Never infer a printf format solely from how the typedef happens to be defined on the development platform.

**Confidence:** Very high. Merged portability fix authored and merged by John Thacker.
