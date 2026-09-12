# Derived Capability and State Conventions

## Prefer authoritative state over redundant capability flags

If an object's existing registration or configuration data already determines whether a capability is available, derive the capability from that authoritative data rather than storing a second Boolean that must be kept synchronized.

Merged MR !25367 provides a concrete EPAN example. Decode-As support had historically been represented both by the presence of Decode-As description data and by a separate `supports_decode_as` flag. The conversion removes the redundant flag and bases `dissector_table_supports_decode_as()` on `dissector_table->da_descriptions`, while deprecating the now-superfluous `dissector_table_allow_decode_as()` calls.

The benefit is more than reducing one structure member. Redundant state permits impossible combinations—for example, a table claiming Decode-As support while lacking the registration data needed to implement it, or having the registration data while the flag remains unset. When one piece of state is a pure function of another, storing both creates synchronization obligations with no semantic value.

### Practical rule

Before adding or retaining a Boolean such as `supports_*`, `has_*`, or `is_*`, ask whether the answer can be obtained cheaply and unambiguously from the object's canonical registered state. If so, expose a helper that derives the answer. Keep an explicit cached flag only when derivation is materially expensive, when the flag represents a genuinely independent state transition, or when the distinction is part of the API contract.

## Prefer observed negotiation state, but account explicitly for partial captures

When a protocol capability is negotiated on the wire, the dissector should prefer state learned from the negotiation exchange over a redundant user setting when that exchange is present. However, packet captures frequently begin after a session is already established, so the absence of the negotiation packet in the capture is not evidence that the capability was not negotiated.

Merged MR !22369 adds RFC 8654 BGP Extended Message support. Alexis La Goutte and Stig Bjørlykke both questioned whether the extended-message allowance should be derived from the BGP OPEN capability rather than exposed as another preference. The contributor pointed out that an OPEN message may not be present in a partial capture, leaving the dissector unable to know what the peers negotiated. The ultimately merged implementation retains an explicit `Allow BGP Extended Messages` preference so those captures can be interpreted without falsely treating every message over the legacy 4096-octet limit as an error; the review also required a concrete capture containing an extended UPDATE message.

### Practical rule

For negotiated protocol features, distinguish three states rather than collapsing them into a Boolean assumption: capability observed as negotiated, capability observed as not negotiated, and negotiation state unknown because the prerequisite exchange was not captured. Use observed protocol state when available. If useful dissection of the unknown case requires user policy, make that fallback explicit and narrowly scoped rather than pretending that missing handshake traffic proves either outcome.

**Confidence:** High. !22369 was merged after substantive maintainer review specifically discussing capability-derived behavior versus an explicit fallback for captures that omit the OPEN exchange.