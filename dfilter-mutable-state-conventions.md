# Wireshark Display-Filter Mutable-State Conventions

## Do not cache derived representations across mutable syntax-tree state without invalidation

Merged master MR !6443, authored by João Valverde, fixes stale display/debug strings from syntax-tree nodes that can mutate and change type. The accepted implementation rebuilds the representation each time and retains only the latest owned string needed to satisfy the return-value lifetime.

**Rule:** a derived cache is valid only if every mutation that can change the derived value participates in reliable invalidation. If that contract is difficult to guarantee, recompute the derived representation and manage only its storage lifetime.

## Internal compiler structures should represent true cardinality

Merged master MR !6444 replaces a fixed pending-jump assumption in display-filter code generation with a list because newer expression forms can require a variable number of fixups.

**Rule:** do not encode an accidental fixed cardinality in compiler IR or code-generation bookkeeping when the language construct is semantically variable-length.

**Confidence:** Very high. Both are merged master display-filter changes authored by João Valverde.
