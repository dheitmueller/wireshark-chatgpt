# Wireshark Wire Text Encoding Context Conventions

This file records durable conventions for carrying and applying protocol-declared text encodings. Current upstream source remains authoritative.

## Carry a document-selected charset through every string extraction path

When a format selects its text encoding at the document or message level, treat that encoding as parse context and use it consistently for every inline string, string-table lookup, and derived text value. Do not fall back to raw pointers or a helper whose encoding semantics happen to match only the common case.

Merged master MR !8784, authored by John Thacker, fixes WBXML by resolving the document charset to a Wireshark `ENC_*` value, retaining it as packet-level protocol data, and using `tvb_get_stringz_enc()` for both inline strings and string-table references. The MR explicitly replaces `tvb_format_text()` and `tvb_get_ptr()` paths that ignored the document-selected encoding. It also retains the WBXML default of UTF-8 when no charset is supplied.

Merged master MR !8790 applies the same principle to MMS: protocol-defined Text-string values are decoded as US-ASCII, while Encoded-string-value uses its MIBEnum charset. Merged !8791 and !8770 independently convert nominally ASCII protocol text through encoding-aware helpers before using the strings in columns or fields.

**Implementation rule:** resolve the protocol or document charset at the layer that owns it, propagate that semantic encoding through nested parsing, and materialize text with the matching `tvb_get_string*_enc()` or tree-item API. A raw byte pointer or generic text formatter is not a substitute for a known wire encoding.

## Validate UI-facing text instead of leaking out-of-domain bytes

Merged master MR !8782 fixes Skinny display labels whose protocol domain is ASCII plus a codebook. Bytes outside that domain are replaced rather than copied through as invalid UTF-8. Merged master MR !8792 similarly validates AFS vectorized strings as UTF-8 and repairs invalid input before adding the field.

**Display rule:** text that reaches columns, protocol-tree strings, JSON-like output, or other UTF-8-facing interfaces must first pass through the appropriate encoding conversion or validation path. Preserve malformed input inspectability through replacement/diagnostic behavior rather than emitting invalid UI strings.

**Confidence:** Extremely high. Multiple merged master encoding fixes, most authored by John Thacker, all converging on explicit wire-encoding semantics.
