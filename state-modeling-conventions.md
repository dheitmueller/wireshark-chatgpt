# Wireshark State-Modeling Conventions

This file records durable conventions for representing parser and protocol state extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Represent one semantic multi-state domain with one variable, not coupled booleans

When two booleans jointly encode one logical state, they create combinations that are redundant or impossible and force readers and compilers to reconstruct the invariant. Prefer an enum or equivalent single value that directly names every valid state, including an explicit unknown/not-yet-determined state when needed.

Merged MR !25213, authored and merged by Guy Harris, replaces NTS-KE's separate `direction_determined` and `request` variables with one request/response/unknown direction value. Besides making the control flow clearer, the explicit state model avoids a false “may be used uninitialized” warning from older GCC, which could not infer that `request` had necessarily been assigned whenever `direction_determined` was true.

**Implementation rule:** if variables are only meaningful as a tuple and one combination constrains the validity of another, ask whether they are really one finite-state value. Encode that state directly so invalid combinations are unrepresentable and dataflow is evident to both maintainers and compilers.

**Confidence:** Extremely high. Merged master clarity/portability change authored and merged by Guy Harris.