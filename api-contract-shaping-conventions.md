# Wireshark API Contract-Shaping Conventions

This file records durable conventions for keeping C interfaces aligned with their actual behavior, ownership model, and subsystem boundaries. Current upstream source remains authoritative.

## Remove output/error parameters that an API does not actually produce

A parameter in a public or shared helper advertises a contract. Keeping an unused error-string/output parameter implies that callers should initialize, inspect, and reason about a result the implementation can never provide.

Merged master MR !11843, authored and merged by Guy Harris, removes the `char **err_str` parameter from `append_extcap_interface_list()` because the routine never returns an error string. The accompanying documentation is changed to describe the actual behavior rather than preserving a misleading signature for symmetry with other helpers.

**Implementation rule:** API shape should describe real observable behavior. Delete dead result/error channels rather than leaving them as ceremonial parameters; when a helper gains a real failure result later, add a contract appropriate to that failure at that time.

**Review rule:** inspect output parameters as promises, not just plumbing. If no execution path writes a meaningful result, simplify the API and its callers instead of perpetuating an imaginary contract.

**Confidence:** Extremely high. Merged master API cleanup authored and merged by Guy Harris with the contract mismatch stated explicitly.

## Separate operation status from result data instead of encoding implementation details in the return value

When a lookup/iterator naturally has two outcomes—whether an item was found and what that item is—use a status result plus an output value rather than exposing an internal representation through the return value. This also leaves the API free to change storage representation without changing caller semantics.

Merged master MR !11829, authored by Peter Wu and merged by João Valverde after substantial review, simplifies the manuf APIs so `ws_manuf_iter_next()` and `ws_manuf_lookup()` return `bool` to indicate whether an entry was found while propagating the matched data through output parameters. The same MR removes pointer-identity comparisons for string values: equal strings may or may not share an address depending on compiler/linker optimization, so pointer equality is not a stable value-semantic test.

**Implementation rule:** use return values for the primary status when that status is logically distinct from returned data, and keep representation details behind the API boundary. Compare strings and other value objects by their defined value semantics unless pointer identity itself is explicitly the contract.

**Review rule:** when an API makes callers interpret a returned pointer/implementation object merely to learn success, consider whether a boolean/status return plus an output result would express the contract more clearly. Treat assumptions about linker string coalescing or object-address identity as non-portable unless the API explicitly guarantees interning.

**Confidence:** Very high. Merged master API simplification by Peter Wu with sustained João Valverde review and explicit discussion of why string-pointer identity is unreliable.

## Inject application-specific discovery behind a callback instead of coupling shared option code to one frontend

Common command-line or library code should not directly depend on one application's concrete discovery implementation when multiple frontends need the same parsing/control flow. Pass the operation needed by the shared layer as a callback or otherwise isolate the application-specific dependency.

Merged master MR !11860, authored and merged by Guy Harris, changes common capture-option handling to receive a callback for fetching the interface list. This avoids hard-wiring the common code to a particular `capture_interface_list()` implementation and lets the separate programs supply their own discovery behavior while retaining one capture-option path.

**Architecture rule:** shared option/control logic may define *when* discovery is needed without owning *how* a particular frontend performs it. Prefer an explicit callback/interface over conditional calls into application-specific code or duplicate shared logic in each executable.

**Confidence:** Extremely high. Merged master architecture cleanup authored and merged by Guy Harris.
