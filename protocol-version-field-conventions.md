# Wireshark Protocol Version Field Conventions

This file records durable conventions for fields whose wire position changes meaning across protocol versions. Current protocol specifications and upstream dissectors remain authoritative.

## Gate field meaning and downstream parsing on the negotiated protocol version

A fixed byte position can change semantic meaning between protocol revisions. The dissector must not expose the newer interpretation merely because the bytes are physically present, and must not let invalid values in an older reserved field activate parsing that only exists in the newer protocol.

Merged master MR !15610 was authored, approved, and merged by Guy Harris and fixes SMB2 Negotiate Protocol handling before SMB 3.1.0. In older requests, bytes later used for `NegotiateContextOffset`, `NegotiateContextCount`, and `Reserved2` belonged to a must-be-zero/must-be-ignored `ClientStartTime` region; older responses similarly used reserved fields where later revisions carry context count/offset. The accepted dissector selects field names and meaning from the dialect actually offered/selected. If an older-version reserved value is nonzero, it reports the malformed condition and forces the internal context offset/count to zero so those bytes cannot accidentally trigger 3.1.0 context dissection.

**Implementation rule:** determine the applicable protocol revision before interpreting version-dependent fields. Represent the field according to that revision, validate older reserved/MBZ forms as specified, and keep invalid reserved bytes from influencing later control flow. Reporting a malformed value and then continuing to consume it as a newer-version length, count, or offset is unsafe.

**Review/testing rule:** for every version transition that repurposes bytes, test both sides of the transition, including malformed older packets with nonzero reserved bytes. Verify both protocol-tree presentation and parser control flow: the older packet should be diagnosed without entering the newer-version substructure parser.

**Confidence:** Exceptional. Merged master correctness change authored and merged by Guy Harris, with the version-specific wire semantics and defensive control-flow behavior documented directly in the MR.