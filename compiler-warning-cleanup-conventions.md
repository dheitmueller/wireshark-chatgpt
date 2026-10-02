# Wireshark Compiler-Warning Cleanup Conventions

## Fix the semantic type mismatch, not merely the warning text

Compiler warnings often expose a real disagreement between the value domain and the types chosen by the code. Suppressing the warning with a convenient cast can hide precision loss, sentinel loss, or unnecessary mutation of otherwise-const data.

Merged master MR !1524 received detailed Pascal Quantin review. A warning fix that cast `nstime_to_sec()` from `double` down to `guint32` was redirected so the integer retransmission timer was converted to `double`, preserving the time value's precision. Pascal also questioned removal of `const`, suggested `guint16` for values populated by `tvb_get_ntohs()`, and repeatedly favored the least invasive signedness conversion compatible with the APIs.

Merged master MR !1546 supplies a complementary warning. The patch changed DRB offsets from signed to unsigned because offsets are ordinarily non-negative. Anders Broman cautioned that Wireshark APIs commonly use signed offsets and some helpers use `-1` as "not found"; mechanically changing signedness can therefore erase a legitimate sentinel contract.

**Implementation rule:** resolve a warning by identifying the semantic domain first: precision, width, signed sentinel values, constness, and ownership. Prefer a narrow conversion at the interface boundary over changing the internal representation merely to satisfy one caller or compiler.

**Review rule:** warning-only MRs deserve semantic review. For every cast or type change, ask which side of the conversion owns the true domain and whether the new type can represent all documented success and sentinel values.

**Confidence:** Very high. Merged fixes with substantive Pascal Quantin and Anders Broman review, and the accepted code reflects the semantic-domain corrections rather than warning suppression.
