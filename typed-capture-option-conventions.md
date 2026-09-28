# Wireshark Typed Capture-Option Conventions

## Consume the typed option model after the capture layer has parsed it

Merged MR !6380 fixes a frame-verdict crash by replacing local interpretation of a generic byte value with the typed packet_verdict_opt_t representation already supplied by the capture-option layer. Numeric TC/XDP verdicts come from typed integer members; byte-oriented verdicts use the typed byte container.

**Rule:** when Wiretap or another lower layer has already validated and normalized a capture option into a discriminated/typed representation, downstream dissectors and UI code should consume that representation. Do not separately reinterpret the original raw byte storage.

**Reasoning:** duplicate decoding splits ownership of validation, byte order, length checks, and type discrimination. The two interpretations can drift and turn malformed or extended options into consumer crashes even though the authoritative parser understood them.

**Confidence:** High. Merged crash fix with a direct change from raw to typed option access.
