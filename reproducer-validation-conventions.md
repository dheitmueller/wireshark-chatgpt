# Reproducer and Protocol-Validity Review Conventions

## Verify the target branch and the input before changing parser behavior

Closed MR !10542 proposed making an 802.11 association response more permissive. Alexis La Goutte requested a representative capture and specification check; the contributor then discovered that current master already behaved correctly and that the reported problem applied only to an older build.

Closed MR !10525 proposed accepting a one-byte NGAP transparent container. Pascal Quantin challenged whether that encoding was valid and supplied the specification context. The contributor subsequently confirmed that the packet had been produced by a faulty test tool, so the permissive parser change was abandoned.

**Review rule:** before changing a dissector for a reported failure, reproduce it against the actual target branch and establish whether the triggering packet is valid according to the governing specification or other authoritative evidence. A stale release, stale local build, or broken generator is not sufficient evidence that current master should accept a new wire form.

**Submission rule:** attach or otherwise preserve the reproducing capture when possible, state the target revision used for reproduction, and call out whether the packet is known-valid, known-invalid, or uncertain.

**Confidence:** High. Both source MRs were closed rather than merged, so they are negative review evidence; the guidance is strengthened by direct review from Alexis La Goutte and Pascal Quantin and by the concrete disproving evidence in each case.

## Treat a representative capture as primary reproducer evidence

For non-trivial packet-analysis bugs, screenshots and prose do not replace the packet data needed to reproduce the behavior. A representative capture is also valuable for protocol enhancements because reviewers can exercise the actual dissector path and preserve the case for regression testing.

Merged master MR !10412 updates Wireshark's issue templates to state that a capture file is essential for non-trivial bugs and that screenshots are not enough. The feature template similarly encourages a capture suitable for testing the requested behavior. Merged new-dissector MR !10405 independently shows Alexis La Goutte requesting both a pcap and fuzzing evidence before merge; the contributor supplied both.

**Review/submission rule:** request and preserve a representative capture for non-trivial dissector bugs and enhancements whenever possible. Treat logs, screenshots and prose as supplemental context rather than substitutes for a reproducible capture.

**Confidence:** Very high. The expectation is encoded in merged project templates and independently exercised in merged dissector review.

