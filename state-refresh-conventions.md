# Wireshark State Refresh Conventions

This file records durable conventions for replacing derived or cached state during redissection, preference changes, and other refresh operations. Current upstream implementation remains authoritative.

## Clear stale derived state before conditionally installing its replacement

When a refresh/recompute operation can legitimately produce no replacement value, old derived state must be cleared unconditionally before the new result is conditionally stored. Clearing only when a new value exists allows stale data from the previous computation to survive precisely when the correct new state is empty.

Merged MR !24629 fixes packet-frame color-filter details after recolorization. The previous path replaced cached `proto_data` only when new color matches existed; when a rule change caused a packet to stop matching, the old match list remained visible. The accepted fix always removes the old per-frame data first and only then stores the newly computed matches when present. The same MR also removes a derived `has_color_info` boolean whose truth could diverge from the nullable primary `color_filter`, letting each display path guard the object it actually consumes.

**Implementation rule:** model refresh as replacement, not incremental success-only assignment: invalidate the previous derived result first, recompute, then publish the new result if one exists. Avoid redundant validity flags when the actual pointer/container state can serve as the authoritative condition.

**Confidence:** High. Merged master correctness fix, with the stale-state failure mode and the simplification of redundant validity state explicitly described and reviewer-driven.

## Invalidate caches together when one cache contains references into another

Caches that are logically independent at the lookup layer can still share one lifetime when one stores pointers or derived records backed by the other. Invalidating only the source cache leaves the dependent cache holding stale references; invalidating only the dependent cache can preserve results derived from obsolete configuration.

Merged master MR !16350 caches Signal PDU value-name configuration to avoid repeated hash-table lookups. The MR explicitly notes that `data_spdu_signal_list` and `data_spdu_signal_value_names` must be invalidated at the same time because the former retains references into the latter. The accepted implementation rebuilds the related structures as one refresh operation.

**Implementation rule:** identify reference relationships between caches, not just their lookup keys. If cache B contains pointers, indexes, or derived state whose validity depends on cache A, give them a common invalidation boundary and rebuild order rather than allowing either to refresh independently.

**Review rule:** performance-motivated caching changes require a lifecycle audit. Ask what configuration or source-state changes invalidate the cached result, whether the cached object owns or borrows referenced data, and which other caches must be cleared or rebuilt atomically with it.

**Confidence:** High. Merged master optimization with the coupled-lifetime requirement stated explicitly in the MR rationale and embodied in the accepted implementation.
