# Wiretap Packet Option Model Conventions

## Reuse the existing generic packet-option representation

Closed MR !2823 attempted to support multiple packet comments by adding a new generic TLV-list abstraction alongside Wiretap's block-option machinery. Guy Harris explicitly pointed to `wiretap/wtap_opttypes.{c,h}` and recommended using the existing mechanism instead. The contributor closed that design in favor of merged master MR !2859, which carries packet comment/verdict state through a `wtap_block`. During !2859, Guy further stated that once the packet record carries the block, packet options should be represented in that option array and duplicate dedicated fields should disappear.

**Architecture rule:** when a subsystem already has an extensible metadata/options abstraction, extend that abstraction rather than introducing a second general-purpose container for the same semantic class. The generic representation should become authoritative; duplicated special-case members create divergent ownership and serialization paths.

**Review rule:** before inventing a new generic metadata container, look for an existing subsystem-level option/extension mechanism. A closed prototype can provide useful negative evidence when a merged successor adopts the maintainer-requested abstraction.

**Confidence:** extremely high. Direct Guy Harris architecture guidance in !2823 and !2859, with the requested direction represented by a merged master successor.
