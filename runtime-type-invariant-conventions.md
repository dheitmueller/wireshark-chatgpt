# Runtime Type-Invariant Conventions

This file records durable conventions for deciding when Wireshark should rely on compile-time type information versus runtime magic/type tags. Current upstream source remains authoritative.

## Do not duplicate compile-time-known wrapper types with redundant runtime magic

If a wrapper object has exactly one concrete representation and the compiler already enforces that representation, a second magic value on the wrapper adds state and checks without adding meaningful type discrimination. Reserve runtime magic/type tags for payloads whose concrete representation is genuinely erased or selected dynamically.

Merged master MR !12394, authored and merged by João Valverde, removes `STNODE_MAGIC` from display-filter syntax nodes because `stnode_t` itself has only one wrapper type. The change deliberately keeps the magic values on the dynamically typed data stored inside syntax nodes. Removing the redundant wrapper magic exposed an existing assertion bug in `sttype_oper_get_op()`: the function had been checking `OPER_MAGIC` against the outer `stnode_t *` instead of first obtaining the `oper_t *` payload. The accepted fix retrieves the payload with `stnode_data(node)` and applies `ws_assert_magic()` to that actual representation.

**Implementation rule:** use the language's static type system for invariants it can express. Add runtime tags or magic values where an API erases the concrete type, stores one of several payload representations behind a generic pointer, or otherwise needs runtime discrimination.

**Assertion rule:** when a runtime magic value belongs to a payload representation, perform the assertion on the payload object that owns that magic, not on an enclosing wrapper whose address merely happens to lead to it.

**Review rule:** when removing a redundant type tag, audit neighboring assertions rather than mechanically deleting checks. Redundant outer checks can mask assertions that were accidentally aimed at the wrong representation; simplifying the type model is an opportunity to expose and correct those mistakes.

**Confidence:** High. Merged master core display-filter cleanup authored and merged by João Valverde; the simplification directly exposed and fixed a real mis-targeted runtime assertion.
