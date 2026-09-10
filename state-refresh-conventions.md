# Wireshark State Refresh Conventions

This file records durable conventions for replacing derived or cached state during redissection, preference changes, and other refresh operations. Current upstream implementation remains authoritative.

## Clear stale derived state before conditionally installing its replacement

When a refresh/recompute operation can legitimately produce no replacement value, old derived state must be cleared unconditionally before the new result is conditionally stored. Clearing only when a new value exists allows stale data from the previous computation to survive precisely when the correct new state is empty.

Merged MR !24629 fixes packet-frame color-filter details after recolorization. The previous path replaced cached `proto_data` only when new color matches existed; when a rule change caused a packet to stop matching, the old match list remained visible. The accepted fix always removes the old per-frame data first and only then stores the newly computed matches when present. The same MR also removes a derived `has_color_info` boolean whose truth could diverge from the nullable primary `color_filter`, letting each display path guard the object it actually consumes.

**Implementation rule:** model refresh as replacement, not incremental success-only assignment: invalidate the previous derived result first, recompute, then publish the new result if one exists. Avoid redundant validity flags when the actual pointer/container state can serve as the authoritative condition.

**Confidence:** High. Merged master correctness fix, with the stale-state failure mode and the simplification of redundant validity state explicitly described and reviewer-driven.
