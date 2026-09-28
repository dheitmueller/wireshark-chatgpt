# Composite TVBuff Lifecycle Conventions

Create a composite TVBuff only when there is a component to append. Merged MR !9359 provides the accepted example.

## Prefer one owned output buffer when decompressor chunks are transient construction state

Merged !9273 replaces the Zstandard helper's per-chunk real-data TVBuffs plus composite construction with one growable owned output buffer. On success it creates one real-data TVBuff and transfers ownership with a free callback; on failure it frees the raw output buffer directly. The MR also adds an explicit successful zero-output decompression test.

**Implementation rule:** when streaming-library chunks have no independent semantic lifetime, one final owned output buffer can be simpler and safer than a composite of separately owned temporary TVBuffs.

**Testing rule:** exercise successful zero-output input explicitly. Empty decompressed data can be a valid result and should not collapse into the same state used for decompression failure.

This complements !9359's rule to instantiate a composite TVBuff only when a component is actually available.

**Confidence:** High. Merged core-TVBuff fix with an explicit regression test for the empty-output edge case.

## Localize optional-empty handling instead of changing a foundational constructor contract casually

Merged master MR !6250 makes composite TVBuff append/prepend operations ignore a NULL or zero-length member. Closed MR !6249 had instead proposed making zero-length subset constructors return NULL globally. After Developer Den discussion, the author abandoned that design because every constructor caller would have to be audited for the new NULL return and because a zero-length request can reflect several different situations: malformed input, an unimplemented case, or a genuine dissector bug.

**Implementation rule:** if an aggregation boundary can safely treat an absent/empty component as a no-op, prefer that narrow behavior over introducing a new sentinel return into a widely used constructor. Changing a foundational constructor to return NULL for a previously representable value requires a caller-by-caller contract audit and a clear semantic reason for collapsing that value into absence.

**Evidence note:** !6250 is the accepted merged implementation. !6249 is used only as negative design-history evidence explaining why the broader contract change was rejected.

**Confidence:** High. Accepted master fix directly contrasted with its abandoned global alternative.
