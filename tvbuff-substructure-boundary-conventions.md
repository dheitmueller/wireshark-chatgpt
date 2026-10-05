# TVBuff Substructure Boundary Conventions

Current upstream source remains authoritative.

## Give child parsers a TVBuff bounded to their semantic region

When a helper owns one length-delimited semantic region of a packet, prefer to pass a subset TVBuff whose bounds are exactly that region instead of passing the full parent TVBuff plus a separate caller-managed start/end contract. This places the containment invariant in the TVBuff itself: the child parser can use local offsets, obtain its natural end from the subset length, and rely on normal TVBuff bounds behavior if it attempts to consume bytes belonging to a following sibling or extension.

Merged master MR !10301, authored by Guy Harris, refactors NHRP this way. The caller creates a `tvb_new_subset_length()` for the Mandatory Part; `dissect_nhrp_mand()` begins at local offset zero and derives its end from `tvb_reported_length(tvb)` instead of receiving the full NHRP packet, a pointer to the parent's offset, and a separate mandatory-part length.

**Implementation rule:** model a protocol-defined bounded child region as a bounded TVBuff when practical. Keep parent cursor advancement in the parent and let the child parser operate in its own coordinate and bounds domain.

**Review rule:** when a helper accepts a full packet TVBuff together with explicit end/length bookkeeping for one child structure, consider whether a subset TVBuff would express the ownership boundary more directly and eliminate duplicated range invariants.

**Confidence:** Extremely high. Merged master architecture work authored by Guy Harris.
