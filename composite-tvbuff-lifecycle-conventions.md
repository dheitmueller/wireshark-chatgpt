# Composite TVBuff Lifecycle Conventions

Create a composite TVBuff only when there is a component to append. Merged MR !9359 provides the accepted example.

## Prefer one owned output buffer when decompressor chunks are transient construction state

Merged !9273 replaces the Zstandard helper's per-chunk real-data TVBuffs plus composite construction with one growable owned output buffer. On success it creates one real-data TVBuff and transfers ownership with a free callback; on failure it frees the raw output buffer directly. The MR also adds an explicit successful zero-output decompression test.

**Implementation rule:** when streaming-library chunks have no independent semantic lifetime, one final owned output buffer can be simpler and safer than a composite of separately owned temporary TVBuffs.

**Testing rule:** exercise successful zero-output input explicitly. Empty decompressed data can be a valid result and should not collapse into the same state used for decompression failure.

This complements !9359's rule to instantiate a composite TVBuff only when a component is actually available.

**Confidence:** High. Merged core-TVBuff fix with an explicit regression test for the empty-output edge case.
