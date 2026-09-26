# Wireshark Behavior-Preserving Refactor Validation

This file records durable validation practices for refactors intended to change structure without changing dissection behavior. Current upstream tests and review requirements remain authoritative.

## Compare externally visible dissector output across representative captures

A large parser/dissector refactor can compile cleanly while changing protocol-tree structure, field values, expert information, or ordering. When the stated intent is behavior preservation, use an output-level comparison across a representative capture set rather than relying only on code inspection.

Merged master MR !8852 refactors PCEP object dispatch from three long, mostly duplicated switch statements into a table of object-specific dissector functions with a common signature. The author validated the change by running `tshark -nVr` before and after the refactor on captures from issues #15834, #13687, #11782, #11213, #11046, #6995, and #5626 and comparing the decoded output.

**Testing rule:** for a behavior-preserving dissector refactor, assemble captures that exercise the affected variants and compare a stable textual or structured decode before and after. Prefer a corpus broad enough to cover the switch/table cases being consolidated.

**Review rule:** distinguish structural goals from semantic goals. If the MR is supposed to be a pure refactor, unexplained output differences are regressions or must be called out explicitly rather than hidden inside the cleanup.

**Confidence:** Very high. Merged master refactor with its validation corpus and comparison method documented directly in the MR description.
