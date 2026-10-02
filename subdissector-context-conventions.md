# Subdissector Context Conventions

Merged SOME/IP MR !1710 passes a small context structure to child dissectors containing the parent service ID, method ID, message type, and major version. The child payload and table selector did not contain enough information by themselves to identify the parent message semantics.

**Rule:** when child decoding depends on parent-header information outside the child payload or registration key, pass that information explicitly through the dissector call context. Treat the context structure as part of the extension-point contract rather than requiring child dissectors to recover parent state indirectly.

**Confidence:** High. Merged master change whose stated purpose is to make SOME/IP subdissectors usable.

## Do not duplicate TVB length state in the child context

A child context should carry semantic information that is not already represented by the child TVB. Duplicating captured/reported lengths in a parallel structure creates two sources of truth and can make truncation handling inconsistent.

Merged PDU Transport MR !1327 initially passed the declared PDU length to subdissectors in its context structure. During review, Jaap Keuter pointed out that this is already represented by the TVB through its length and reported length. The contributor removed the redundant length; the final context carries the semantic PDU ID, while truncation remains a TVB property.

**Rule:** use the dissector context for parent semantics that the child payload or dispatch key cannot supply. For byte availability and truncation, rely on the bounded TVB and its captured/reported-length contract unless the protocol genuinely defines some additional independent length semantic.

**Confidence:** Very high. Direct Jaap Keuter review was incorporated before the master MR merged.
