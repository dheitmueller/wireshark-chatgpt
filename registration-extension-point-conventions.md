# Wireshark Registration Extension-Point Conventions

This file records durable conventions for protocol extension-point registration. Current upstream source remains authoritative.

## Create extension-point registries before the handoff phase that consumes them

Dissector tables, taps, and similar registries are identities that other protocols may consume during their handoff functions. Creating such an extension point in the owning protocol's handoff makes correctness depend on relative handoff order: another dissector can legitimately attempt to attach before the table exists.

Merged master MR !15568 moves the TZSP encapsulation dissector-table registration from `proto_reg_handoff_tzsp()` into `proto_register_tzsp()`. Its stated purpose is to guarantee that the table already exists for every other dissector that may hook into it from its own handoff. John Thacker approved and merged the change. Independently, merged master MR !16252 moves tap registrations from handoff into protocol registration across multiple dissectors for the same cross-dissector ordering reason.

**Architecture rule:** establish extension-point identities in protocol registration when peer handoff routines are expected to bind to them. Reserve handoff for installing bindings, resolving already-registered dependencies, and applying preferences; do not create a registry there if another handoff may need the registry first.

**Review rule:** when a new dissector table, tap, or equivalent registry is introduced, search for both producers and consumers. If consumption can occur from a different protocol's handoff, registration order must not depend on the incidental ordering of handoff callbacks. Generated/template sources must preserve the same lifecycle placement.

**Confidence:** Extremely high. The TZSP fix is a merged master change explicitly justified by cross-dissector availability and approved/merged by John Thacker, and the same lifecycle rule is independently demonstrated by the broader accepted tap-registration cleanup in !16252.

## Put profile-specific payload semantics behind a generic extension point

A generic/base protocol should not hard-code the payload interpretation of one profile when the base protocol only establishes an extension field or opaque option. Expose a dissector table or equivalent dispatch point at the generic layer so each profile can supply its own interpretation and users can select among legitimate meanings with Decode As when necessary.

Merged master MR !15122, authored and merged by Guy Harris, changes the OSI CLNP/ES-IS security-option path from direct ICAO ATN-specific coupling to a Decode-As dissector table. The MR explicitly notes that this removes entanglement among the CLNP dissector, generic OSI-options code, and the ATN security-option dissector while also permitting other OSI profiles to register their own security-option decoder.

**Architecture rule:** keep the base dissector responsible for the generic wire structure and extension boundary; put profile/application-specific payload interpretation behind a registered subdissector mechanism. Prefer this to adding profile tests and dependencies inside the generic parser.

**Review rule:** when a change teaches a generic dissector about one named downstream profile, ask whether the base protocol actually assigns that interpretation or merely carries opaque/profile-defined data. If the latter, look for a dissector table, heuristic list, or Decode-As boundary instead of direct coupling.

**Confidence:** Extremely high. The accepted design is a merged master architectural change authored and merged by Guy Harris, and it turns a profile-specific special case into an explicit reusable extension mechanism.

## Place shared extension points at their semantic owner

A dissector table or context structure that represents a protocol-independent standard namespace should not remain owned by whichever protocol happened to use it first. Put the registry and its caller contract in a neutral component whose names describe the shared semantic domain.

Merged master MR !10371, authored by Guy Harris, decouples the `media_type` dissector table from HTTP. Internet media types are used by HTTP, SIP, CoAP and other carriers, so the accepted change moves table registration and the data contract out of `packet-http.[ch]` into `packet-media-type.[ch]` and renames HTTP/message-specific types to media-container/content terminology.

**Architecture rule:** choose the owner, public header, type names and registration location of a shared extension point from the semantic namespace it models. If unrelated carriers use the same dispatch namespace, extract it from a carrier-specific dissector rather than making those callers depend on the historical first implementation.

**Confidence:** Extremely high. Merged master architectural refactor authored by Guy Harris.

