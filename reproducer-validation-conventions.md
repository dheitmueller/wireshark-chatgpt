# Reproducer and Protocol-Validity Review Conventions

## Verify the target branch and the input before changing parser behavior

Closed MR !10542 proposed making an 802.11 association response more permissive. Alexis La Goutte requested a representative capture and specification check; the contributor then discovered that current master already behaved correctly and that the reported problem applied only to an older build.

Closed MR !10525 proposed accepting a one-byte NGAP transparent container. Pascal Quantin challenged whether that encoding was valid and supplied the specification context. The contributor subsequently confirmed that the packet had been produced by a faulty test tool, so the permissive parser change was abandoned.

**Review rule:** before changing a dissector for a reported failure, reproduce it against the actual target branch and establish whether the triggering packet is valid according to the governing specification or other authoritative evidence. A stale release, stale local build, or broken generator is not sufficient evidence that current master should accept a new wire form.

**Submission rule:** attach or otherwise preserve the reproducing capture when possible, state the target revision used for reproduction, and call out whether the packet is known-valid, known-invalid, or uncertain.

**Confidence:** High. Both source MRs were closed rather than merged, so they are negative review evidence; the guidance is strengthened by direct review from Alexis La Goutte and Pascal Quantin and by the concrete disproving evidence in each case.
