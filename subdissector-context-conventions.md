# Subdissector Context Conventions

Merged SOME/IP MR !1710 passes a small context structure to child dissectors containing the parent service ID, method ID, message type, and major version. The child payload and table selector did not contain enough information by themselves to identify the parent message semantics.

**Rule:** when child decoding depends on parent-header information outside the child payload or registration key, pass that information explicitly through the dissector call context. Treat the context structure as part of the extension-point contract rather than requiring child dissectors to recover parent state indirectly.

**Confidence:** High. Merged master change whose stated purpose is to make SOME/IP subdissectors usable.
