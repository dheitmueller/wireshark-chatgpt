# Wireshark Tap Message-Boundary Conventions

## Export the logical message slice, not the enclosing TVBuff prefix

Merged master MR !6454, authored by John Thacker, fixes HTTP follow-tap and fallback-data behavior when `dissect_http_message()` is invoked at a nonzero offset because one TVBuff contains multiple logical messages. The accepted code remembers the invocation's original offset and creates downstream subsets beginning there rather than at offset zero. Stable backports !6459 and !6460 carry the same fix.

**Rule:** a tap or fallback dissector should receive the bytes belonging to the logical object it represents. Sharing storage with a larger TVBuff does not make bytes before the current message part of that tap event.

**Review rule:** test multi-message or multi-segment frames where the dissector starts at a nonzero offset; single-message captures can hide an incorrect assumption that every logical object begins at TVBuff offset zero.

**Confidence:** Extremely high. Merged master fix authored by John Thacker with two merged stable backports.
