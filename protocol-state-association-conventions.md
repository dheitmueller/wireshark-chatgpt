# Protocol State Association Conventions
Merged MR 6000 contains direct John Thacker review on cross-frame state. Per-frame proto data is for facts attached to one frame; conversation data is for facts belonging to a conversation. A capture-wide global is wrong when independent relationships can interleave. If related traffic does not share the ordinary transport conversation, use the relationship the protocol itself uses: a suitable conversation endpoint/key or an internal keyed structure.

Rule: choose persistent-state identity from protocol semantics before choosing a storage API. Test with at least two interleaved independent relationships so accidental global coupling is visible.

Confidence: very high; merged master change with explicit John Thacker architecture review.
