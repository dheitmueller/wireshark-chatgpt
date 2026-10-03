# Protocol association state conventions

## A conversation may contain multiple independent protocol associations

Merged master MR !272 fixes PROFINET state for devices with multiple simultaneous Application Relationships (ARs). Reusing one station object for the whole MAC conversation caused IOCS/IOData from different ARs to be decoded against the wrong state. The accepted implementation records AR UUID, input/output frame IDs, and setup/release frame numbers, then selects the active association before retrieving its station state. Pascal Quantin also reviewed `PINFO_FD_VISITED`, NULL handling, and file-scope allocation/reset behavior.

**Keying rule:** do not assume transport or link-layer conversation identity is the final state key. If a protocol multiplexes independent associations on one conversation, include the protocol association identity and, when needed, its validity interval.

**Lifecycle rule:** association lookup, allocation, and reset should make the state lifetime obvious and should align with the allocator scope used to retain that state.

**Confidence:** High. Merged master correctness change with substantive Pascal Quantin review.
