# Encapsulation dispatch conventions

## Do not accidentally suppress a mandatory payload handoff

During review of merged master MR !289 (IEEE 802.1CB R-Tag), Jaap Keuter rejected a condition that could prevent the following dissector from being called and emphasized that the encapsulated protocol must still be dispatched. The accepted code always builds the EtherType handoff context and invokes the EtherType dissector after parsing the tag. The same review removed preference/template code that had no semantic role in R-Tag and corrected a pseudo-header parameter that was incorrectly marked unused.

**Dispatch rule:** once an outer protocol has established that encapsulated payload exists and has a discriminator for it, unrelated validation, reserved-field, or preference conditions must not silently suppress the normal downstream handoff. If malformed input makes dispatch unsafe, that must follow from the payload-boundary/discriminator contract itself.

**Template rule:** when basing a dissector on a related one, audit every preference, parameter annotation, branch, and helper for the new protocol's semantics. Copy/paste similarity is not evidence that the control flow remains valid.

**Confidence:** Very high. Direct Jaap Keuter review incorporated into a merged master new dissector.
