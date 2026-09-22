# Wireshark Validation Availability Conventions

This file records durable conventions for distinguishing invalid user input from cases where Wireshark lacks enough context to validate the input. Current upstream source remains authoritative.

## Distinguish “cannot validate” from “validation failed”

A validator should only report an input as invalid when the prerequisites needed to perform that validation are actually available. Missing or unknown semantic context is a third state, not evidence that the input itself is wrong.

Merged master MR !13317, authored by John Thacker, fixes capture-filter checking when a selected interface has `active_dlt == -1`, a sentinel used when permissions prevent discovery or libpcap/WinPcap/Npcap does not know the link-layer type. The previous path attempted to compile the capture filter anyway and produced a cryptic error. The accepted behavior instead uses the same warning state as user-defined/unknown DLTs and reports that Wireshark is unable to check the capture filter. Merged release backports !13323, !13324, and !13325 preserve the same behavior.

**Implementation rule:** identify the context required by a validator before invoking it. If that context is unknown or unavailable, return/present an explicit indeterminate or unavailable result rather than manufacturing a validation failure from a call that could not have been meaningful.

**UI rule:** user-facing diagnostics should preserve this distinction. “Unable to check” is materially different from “invalid filter”: the former describes Wireshark's current knowledge/capability, while the latter makes a claim about the user's input.

**Confidence:** Very high. Merged master behavior authored by John Thacker and carried to three supported release branches.

## Cryptographic validation requires the complete authenticated byte region

Authentication/checksum-style validation is meaningful only when every byte covered by the calculation is present. A truncated capture must not be reported as an authentication failure merely because Wireshark cannot reconstruct the bytes that would have been hashed or MACed.

During review of merged MR !13013, which adds RADIUS Message-Authenticator validation, Pascal Quantin explicitly required checking that the complete authenticator region exists before reading/copying it and warned against running Message-Authenticator validation when `tvb_reported_length() != tvb_captured_length()` (or without an equivalent `tvb_bytes_exist()` availability check). The same review also noticed that analogous bounds/availability issues existed in the pre-existing authenticator-validation path, so the safety rule applies to existing and new validation code alike.

**Bounds rule:** before a fixed-offset read or copy used by a validator, prove that the entire requested region is captured. Validate `offset + length` safely or use the tvbuff availability helpers rather than assuming protocol-declared structure implies captured bytes.

**Semantic rule:** distinguish three outcomes: validation succeeded, validation failed with complete required data, and validation was unavailable because required bytes/context were not captured. Truncation belongs in the third category.

**Review implication:** when adding a cryptographic/message-integrity validator, test both malformed short inputs and capture truncation in addition to valid and invalid authenticators. Do not let a security-oriented validator introduce an out-of-bounds read or a false-negative diagnosis.

**Confidence:** Very high. The requirements came from direct Pascal Quantin review of a merged master authentication feature and were incorporated before merge.