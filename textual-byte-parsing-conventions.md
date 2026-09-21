# Wireshark Textual Byte Parsing Conventions

This file records durable conventions for converting textual byte representations into binary data. Current upstream APIs remain authoritative.

## Reject structurally malformed byte strings instead of inventing implicit padding

A parser for hexadecimal byte text should not silently reinterpret an invalid digit grouping by inserting a zero nibble in an arbitrary position. If a representation does not satisfy the parser's byte-grouping contract, reject it unless that exact shorthand is deliberately supported and unambiguous.

Merged master MR !14152, authored by John Thacker and merged by Anders Broman, fixes `hex_str_to_bytes()` accepting odd runs of three or more hex digits. The old code consumed pairs and then treated the final leftover digit as a byte by itself, so text such as `3.109.209.43` acquired an implicit leading zero on the *last* digit of each odd run and decoded to an unexpected byte sequence. The accepted implementation rejects such runs while retaining the explicitly supported single-hex-digit form. Merged release-4.2 backport !14153 carries the same behavior to the stable branch.

**Implementation rule:** define the accepted grouping and separator grammar for textual bytes and reject inputs that do not conform. Do not make malformed multi-digit groups parseable by implicit padding or another recovery rule unless that recovery is an intentional, documented part of the format.

**Review rule:** include negative tests for odd digit counts, changing separators, missing separators, and boundary cases around any allowed shorthand. A parser described as permissive should still avoid ambiguous recovery that maps malformed text to a plausible but unintended byte array.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker, accepted by Anders Broman, and propagated to release-4.2.