# Wireshark Validation Availability Conventions

This file records durable conventions for distinguishing invalid user input from cases where Wireshark lacks enough context to validate the input. Current upstream source remains authoritative.

## Distinguish “cannot validate” from “validation failed”

A validator should only report an input as invalid when the prerequisites needed to perform that validation are actually available. Missing or unknown semantic context is a third state, not evidence that the input itself is wrong.

Merged master MR !13317, authored by John Thacker, fixes capture-filter checking when a selected interface has `active_dlt == -1`, a sentinel used when permissions prevent discovery or libpcap/WinPcap/Npcap does not know the link-layer type. The previous path attempted to compile the capture filter anyway and produced a cryptic error. The accepted behavior instead uses the same warning state as user-defined/unknown DLTs and reports that Wireshark is unable to check the capture filter. Merged release backports !13323, !13324, and !13325 preserve the same behavior.

**Implementation rule:** identify the context required by a validator before invoking it. If that context is unknown or unavailable, return/present an explicit indeterminate or unavailable result rather than manufacturing a validation failure from a call that could not have been meaningful.

**UI rule:** user-facing diagnostics should preserve this distinction. “Unable to check” is materially different from “invalid filter”: the former describes Wireshark's current knowledge/capability, while the latter makes a claim about the user's input.

**Confidence:** Very high. Merged master behavior authored by John Thacker and carried to three supported release branches.