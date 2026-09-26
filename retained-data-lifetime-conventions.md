# Wireshark Retained-Data Lifetime Conventions

This file records durable conventions for pointers, strings, and buffers retained beyond the parsing scope that produced them. Current upstream allocator and API contracts remain authoritative.

## Copy packet-derived strings into the lifetime of retained state

A state object does not extend the lifetime of a pointer merely by storing it. If conversation, transaction, or file-scope state retains a string obtained while parsing a packet, the retained value must have storage whose lifetime is at least as long as the destination state.

Merged master MR !9114, authored and merged by João Valverde, fixes SIP resend tracking after ASAN exposed a lifetime error. The transaction record retained the parsed CSeq method string directly even though that string's storage could be released while the transaction state remained alive. The accepted fix duplicates the method into file scope before assigning it to the retained state.

**Implementation rule:** before storing a pointer in long-lived dissector state, identify who owns the pointed-to memory and how long it remains valid. Duplicate or deep-copy packet-derived strings and buffers into the allocator scope that owns the destination state when the source lifetime is shorter or uncertain.

**Review rule:** audit assignments into file-, conversation-, and transaction-scope structures as ownership boundaries. A simple pointer assignment is safe only when the source storage is guaranteed to outlive every future use of the destination.

**Confidence:** Very high. Merged master memory-lifetime fix authored and merged by João Valverde with a concrete sanitizer-detected failure.
