# Wireshark Comparison-Semantics Conventions

This file records durable rules about equality, identity, and lookup semantics extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Compare strings by value when the invariant is about names, not storage identity

Pointer equality and string equality answer different questions. A list or map API that compares pointers can appear to enforce uniqueness while actually accepting duplicate textual names whenever those names occupy different allocations.

Merged master MR !20337 fixed duplicated plugin main-menu registration. During review, John Thacker identified that an existing assertion used `g_list_find(menubar_menunames, name)`, which compares the stored pointer with `name`; it therefore did not enforce the documented intent that a protocol register only one main-menu name. He explicitly pointed to `g_list_find_custom(..., g_strcmp0)` as the value-comparison form if textual uniqueness is the intended invariant, and also questioned whether the older uniqueness mechanism itself remained necessary after the GTK frontend disappeared.

**Implementation rule:** choose equality according to the semantic identity being modeled. For textual names, identifiers, labels, paths, or other value objects, use value comparison unless allocation identity is intentionally part of the contract. Pointer equality is appropriate only when two references being the exact same object is the invariant.

**Review rule:** when a uniqueness/assertion check appears ineffective, inspect the comparison semantics of the container API before changing surrounding logic. Also revalidate the invariant itself when it originated in an obsolete backend or architecture; fixing the comparison mechanically is not always the same as preserving the right modern behavior.

**Confidence:** Very high. Merged master fix with direct review and diagnosis from John Thacker.