# Wireshark Sorted-Container Search Conventions

This file records durable conventions for ordered-container APIs and their performance-sensitive search behavior. Current upstream source remains authoritative.

## Expose materially different search bias with a clear semantic API

A sorted insertion routine can be correct while still performing badly when its search starts from the end opposite the workload's natural arrival order. When both search directions are useful, keep the ordinary semantic behavior stable and expose the alternative with a descriptive operation name rather than a cryptic abbreviation or an unrelated boolean argument.

Merged master MR !16432 addresses TCP out-of-order segment handling. After a gap, subsequent segments commonly arrive with increasing sequence numbers; repeatedly searching the OOO list from its head can therefore approach quadratic behavior. The accepted change adds `wmem_list_append_sorted()` and uses it for the TCP OOO list so comparison begins from the tail. Review by Stig Bjørlykke and Anders Broman focused on naming and API shape: `append_sorted` was preferred over an ambiguous `_app` suffix, and the author explicitly favored separate minimal semantic operations over adding a boolean mode to the existing function. A capture reproducing the performance case was supplied with the MR.

**Implementation rule:** choose sorted-container traversal direction from the expected locality of the workload, not only from abstract big-O behavior. If callers legitimately need both head-biased and tail-biased insertion, give those operations readable semantic names and preserve the existing default unless changing it is correct for all callers. Avoid mode booleans that make a performance-significant policy invisible at call sites.

**Confidence:** Very high. Merged master performance/API change with a reproducing capture and explicit maintainer review of the interface naming and shape.