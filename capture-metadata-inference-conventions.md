# Capture metadata inference conventions

## Prefer demonstrated packet semantics over ambiguous header labels

Capture-header fields can describe channel configuration rather than the modulation of each packet. Where real captures show ambiguous or inconsistent use, derive packet properties from stronger packet-local evidence and use frequency or channel only to disambiguate cases it can actually distinguish.

Guy Harris's merged !2311 documents a PPI case where channel flags and the packet data rate disagree. The accepted code uses extension headers and data rate to determine modulation and preserves explicit absence for properties the capture did not provide. Merged !2323 applies the same reasoning across several capture formats, and !2332 corrects CommView by deriving modulation from data rate rather than its band field.

**Implementation rule:** rank capture metadata by what its source really guarantees, cross-check ambiguous fields against independent packet evidence, and do not synthesize details that were not captured.

**Confidence:** Extremely high. Repeated merged master changes authored by Guy Harris.


## Additional evidence from !2310 and !2276

Merged master MRs !2310 and !2276, both authored by Guy Harris, reinforce that an 802.11 capture header can describe channel properties without reliably describing the modulation of each packet. The accepted code ranks explicit packet modulation metadata and data rate above ambiguous channel flags, then uses frequency only where it can distinguish otherwise valid possibilities.

**Implementation rule:** infer per-packet properties from evidence that actually applies to the packet. Do not treat an ambiguously populated channel field as authoritative merely because its label resembles the desired property.

**Confidence:** Extremely high. Repeated merged Guy Harris changes.
