# Wireshark Wiretap Text-Parsing Conventions

This file records durable conventions for parsing textual capture/log formats in Wiretap. Current upstream source remains authoritative.

## Use checked conversions that match the destination domain

A text reader must distinguish a valid numeric field from malformed text, overflow, underflow, or a value outside the destination type's domain. Unchecked `sscanf()`-style conversion or conversion followed by narrowing can turn invalid file contents into silently corrupted parser state.

Merged master MR !14713, authored and merged by Guy Harris, hardens the CLLog reader by replacing unchecked conversions with Wireshark checked conversion helpers such as `ws_strtou32()`. The change also widens storage where the format's value domain requires it. Conversion failures become `WTAP_ERR_BAD_FILE` with an explanatory `err_info` instead of silently continuing with a partial or truncated value. Guy's follow-up merged master MR !14714 extends the same direction with additional structural and content validation of CLLog records.

**Implementation rule:** parse external textual numeric fields with checked helpers whose signedness and width match the protocol/file-format domain, and assign the destination only after conversion succeeds. Do not rely on casts or narrower destination storage to enforce a range.

**Error rule:** malformed or out-of-range file contents should be rejected through the Wiretap bad-file path with useful context (`WTAP_ERR_BAD_FILE` and `err_info`) rather than being accepted as zero, wrapped, truncated, or otherwise normalized unintentionally.

**Review rule:** when hardening a text reader, audit all fields in the record and the relationships between them, not just the field that triggered the bug. A parser should validate enough structure and value domain to ensure later offset, length, and allocation logic receives trustworthy values.

**Confidence:** Extremely high. Two consecutive merged master parser-hardening changes authored and merged by Guy Harris, with the accepted code moving explicitly from unchecked conversion to checked Wiretap error handling.