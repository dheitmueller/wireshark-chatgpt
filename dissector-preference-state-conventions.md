# Wireshark Dissector Preference-State Conventions

This file records durable conventions for dissector state that is semantically broader than an optional analysis feature. Current upstream source remains authoritative.

## Do not gate foundational protocol identity state behind an optional analysis preference

Merged release-4.0 MR !9416, with release-3.6 backport !9415, fixes TCP stream numbering when TCP Sequence Number Analysis is disabled. Base sequence numbers had been initialized only inside `tcp_analyze_sequence_number()`, so disabling sequence analysis also prevented state needed to recognize a new TCP conversation that reused the same 5-tuple. The accepted fix moves first-pass base-sequence initialization outside the `tcp_analyze_seq` preference gate while leaving additional sequence-analysis work conditional.

**Implementation rule:** classify each piece of dissector state by all of its consumers before placing it behind a preference. State required for protocol identity, conversation lifecycle, stream numbering, or baseline correctness belongs outside a preference that merely controls optional analysis or presentation.

**Review rule:** when a preference short-circuits a helper, inspect every side effect the helper used to perform. If any side effect feeds consumers that remain active with the preference off, split the foundational state update from the optional analysis.

**Confidence:** Very high. Merged stable-branch correctness fix with parallel backports and a direct user-visible failure mode.


## Master provenance: TCP stream identity is foundational state

Merged master MR !9145 is the original TCP correction later carried by the already-reviewed stable backports !9416 and !9415. It moves base-sequence initialization out of the optional Sequence Number Analysis helper because the same state is required to recognize a new TCP conversation when ports and the 5-tuple are reused.

This provides direct master-branch provenance for the rule above: preference-controlled analysis may consume foundational state, but it must not be the only code path that creates state required for baseline protocol identity or stream numbering.

**Confidence:** Very high. Merged master correctness fix, independently reinforced by two maintained-branch backports.
