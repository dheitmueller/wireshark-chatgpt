# Wireshark Address Representation Conventions

This file records durable conventions for typed address values, representation, and byte-order boundaries. Current upstream source remains authoritative.

## Keep address semantics in typed APIs instead of generic integer accessors

An IPv4 address may occupy 32 bits, but it is not semantically just an unsigned integer. Generic integer getters/setters obscure whether a value is being represented as host-order arithmetic state, network-order wire bytes, or an address object whose storage is meant to be consumed bytewise. Use address-specific APIs and trace the representation expected at every boundary.

Merged master MR !12840 replaces `fvalue_set_uinteger()` / `fvalue_get_uinteger()` support for `FT_IPv4` with dedicated `fvalue_set_ipv4()` / `fvalue_get_ipv4()` APIs carrying `ipv4_addr_and_mask`. Merged master MR !12861, authored by John Thacker, then fixes the Qt address editor after the representation distinction exposed a dotted-quad reversal: the `fvalue_get_ipv4()` result and `set_address()` do not have interchangeable generic-integer byte-order semantics, so the accepted code passes the IPv4 address storage expected by the address API rather than round-tripping through an arbitrary integer.

**Implementation rule:** use typed address getters/setters for `FT_IPv4` and other semantic address values. At conversions between typed values and address APIs, verify whether the callee consumes an integer value, an in-memory byte sequence, host-order state, or network-order state; convert exactly once at that boundary.

**Review rule:** treat casts and `hton*()`/`ntoh*()` calls around addresses as representation boundaries that require justification. A 32-bit type match is not proof that two APIs share a byte-order contract.

Closed draft MR !12844 explored a broader IPv4 type cleanup because the existing typedefs made byte order hard to reason about. It is useful motivation but is deliberately lower-weight than the two merged fixes above.

**Confidence:** Very high. The rule is supported by two merged master changes, including a concrete regression/fix authored by John Thacker; the abandoned draft is used only as secondary motivation.
