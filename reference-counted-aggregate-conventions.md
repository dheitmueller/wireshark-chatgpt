# Wireshark Reference-Counted Aggregate Conventions

This file records durable ownership rules for aggregates whose container and members must survive across API ownership boundaries. Current upstream source remains authoritative.

## Reference both the aggregate and its owned members when reusable state crosses a consuming API

Reference counting an outer container is insufficient when that container owns independently reference-counted elements and the consuming API releases those elements as part of normal close or teardown. Reuse requires preserving the complete ownership graph, not just the top-level collection object.

Merged release-3.6 MR !15660, authored and merged by John Thacker as a backport of the master fix, corrects `editcap` reuse of an initial Decryption Secrets Block array across output-file rotation. `wtap_dump_close()` had freed the passed `GArray`, leaving `editcap` with state it intended to reuse. The accepted API adds `wtap_block_array_ref()` / `wtap_block_array_unref()`, which increment or decrement both the `GArray` reference and every contained `wtap_block_t`; callers take an extra reference before closing when they need the same DSB set for the next dump file.

**Implementation rule:** document whether an API borrows, consumes, references, or frees aggregate inputs. If an aggregate and its elements are independently owned, provide ref/unref helpers that preserve the whole aggregate contract symmetrically. Do not keep a pointer for later reuse after passing it to an API whose close path releases its ownership.

**Review rule:** test lifecycle transitions that reuse configuration/data across repeated open-close cycles, such as file rotation. Single-open tests often miss ownership bugs because the dangling aggregate is never consumed again.

**Confidence:** Very high. John Thacker-authored correctness fix with an explicit ownership contract, carried to a stable branch and tied to multiple concrete editcap bugs.