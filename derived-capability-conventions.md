# Derived Capability and State Conventions

## Prefer authoritative state over redundant capability flags

If an object's existing registration or configuration data already determines whether a capability is available, derive the capability from that authoritative data rather than storing a second Boolean that must be kept synchronized.

Merged MR !25367 provides a concrete EPAN example. Decode-As support had historically been represented both by the presence of Decode-As description data and by a separate `supports_decode_as` flag. The conversion removes the redundant flag and bases `dissector_table_supports_decode_as()` on `dissector_table->da_descriptions`, while deprecating the now-superfluous `dissector_table_allow_decode_as()` calls.

The benefit is more than reducing one structure member. Redundant state permits impossible combinations—for example, a table claiming Decode-As support while lacking the registration data needed to implement it, or having the registration data while the flag remains unset. When one piece of state is a pure function of another, storing both creates synchronization obligations with no semantic value.

### Practical rule

Before adding or retaining a Boolean such as `supports_*`, `has_*`, or `is_*`, ask whether the answer can be obtained cheaply and unambiguously from the object's canonical registered state. If so, expose a helper that derives the answer. Keep an explicit cached flag only when derivation is materially expensive, when the flag represents a genuinely independent state transition, or when the distinction is part of the API contract.
