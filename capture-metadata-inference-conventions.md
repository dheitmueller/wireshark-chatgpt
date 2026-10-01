# Capture metadata inference conventions

## Prefer demonstrated packet semantics over ambiguous header labels

Capture-header fields can describe channel configuration rather than the modulation of each packet. Where real captures show ambiguous or inconsistent use, derive packet properties from stronger packet-local evidence and use frequency or channel only to disambiguate cases it can actually distinguish.

Guy Harris's merged !2311 documents a PPI case where channel flags and the packet data rate disagree. The accepted code uses extension headers and data rate to determine modulation and preserves explicit absence for properties the capture did not provide. Merged !2323 applies the same reasoning across several capture formats, and !2332 corrects CommView by deriving modulation from data rate rather than its band field.

**Implementation rule:** rank capture metadata by what its source really guarantees, cross-check ambiguous fields against independent packet evidence, and do not synthesize details that were not captured.

**Confidence:** Extremely high. Repeated merged master changes authored by Guy Harris.
