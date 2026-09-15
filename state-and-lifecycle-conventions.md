# Wireshark State and Lifecycle Conventions

This file records durable conventions for state identity, callback ownership, and lifecycle boundaries observed in upstream Wireshark review.

## Choose state identity from protocol semantics, not transport convenience

- Do not automatically use Wireshark `conversation_t` objects merely because request/response state is being correlated. First determine whether the transport endpoint tuple is actually part of the protocol's semantic identity.
- Merged MR !19625 is a useful counterexample. Diameter requests and responses can legitimately traverse different addresses/ports with multihomed SCTP or load-shared connections. John Thacker pointed out that creating a separate conversation plus per-conversation trees for every ID pair was both semantically awkward and unnecessarily expensive because the dissector already had a multimap keyed by the Diameter hop-by-hop identifier. The accepted direction was to keep request/reply correlation in protocol-owned state rather than force it through transport conversations.
- Prefer the smallest state structure that directly represents the protocol identity. Use `conversation_t` when endpoint/conversation identity is genuinely useful to the protocol; otherwise a protocol-owned map/table may be clearer and substantially cheaper.
- This complements the dissector convention that, when conversation state *is* appropriate, its endpoint identity must use fields stable for the required lifetime rather than whichever addresses happen to be nearest in the stack.

## Keep failure reporting and process termination at the owning layer

- Initialization callbacks should report success/failure to their caller rather than terminating the process from low-level tap/module code. The layer that owns application policy should decide whether a failed registration is fatal.
- Merged MR !19601 standardized stat-tap initialization callbacks around a boolean success result, reports registration errors through the normal command-line error path, and moves application exit behavior into the stat-tap UI layer. Its rationale explicitly says `exit()` should generally be confined to application-internal policy so behavior remains consistent.
- When termination is required, use the project's defined exit-code constants (for example those in `include/ws_exit_codes.h`) rather than inventing ad-hoc numeric exit values. Martin Mathieson explicitly requested this during !19601 review and the discussion was resolved before merge.
- More generally, keep mechanism and policy separated: leaf code should clean up and return a meaningful failure; the owning application/UI layer decides presentation, exit status, and whether execution can continue.

## Evidence weighting

These rules come from merged master MRs and substantive maintainer review. They should therefore carry more weight than abandoned proposals or unmerged experiments when evaluating future Wireshark changes.
