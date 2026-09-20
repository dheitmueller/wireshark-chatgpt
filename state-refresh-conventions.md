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

## Reset dependent analysis state at protocol lifecycle boundaries

Stateful dissectors often maintain sequence, reassembly, or bearer state in lower protocol layers. When a protocol-defined lifecycle event makes that history invalid, the reset belongs at that semantic event boundary, and all dependent layers and sibling protocol variants must be audited. The reset still has to respect the protocol's event-specific semantics; a blanket reset can be just as wrong as leaving stale state in place.

Merged master MR !16110 fixes LTE analysis after reconnect/reestablishment events by having LTE RRC notify the RLC and PDCP dissectors to discard bearer state when the relevant RRC event occurs. Pascal Quantin's review identified additional event variants and NB-IoT counterparts that needed the same audit, while also pointing out that different events require different PDCP behavior: for example, reestablishment resets SRB/UM state but not all AM state, and resume handling is more complicated. Martin Mathieson expanded the accepted change to the relevant LTE/NB-IoT entry points while deliberately leaving unsupported complex cases as explicit TODOs rather than applying an over-broad reset.

**Implementation rule:** tie state invalidation to the protocol event that changes the identity or validity of the state, then audit every layer and sibling variant that derives state from that lifecycle. Encode the reset set per event according to protocol semantics; do not assume that reconnect, reestablishment, resume, and related variants invalidate identical subsets.

**Review rule:** whenever a state-reset fix is added for one control-plane event, explicitly search for equivalent event variants, alternate radio/protocol modes, and dependent state machines. Also identify cases whose reset semantics differ and keep them out of the change unless they are understood and tested.

**Confidence:** Very high. Merged master state-correctness change authored and merged by Martin Mathieson, with detailed protocol-expert review from Pascal Quantin that directly changed the coverage and reset semantics of the accepted implementation.

## A newly opened capture must start with neutral transient-operation state

Queued work, deferred rescans, lock-related flags, and other transient operation state belong to the capture instance that scheduled them. Opening or replacing a capture establishes a fresh lifecycle; it must not inherit a pending operation from the previous file even when normal shutdown paths are expected to have drained that work.

Merged master MR !15187, authored by John Thacker and merged by Anders Broman, clears `redissection_queued` while opening a new capture. Without that reset, a queued rescan from the prior file could survive into the replacement capture and cause later filtering/redissection decisions to behave as though work were still pending for the new file.

**Implementation rule:** when a resource replacement establishes a new capture-file lifecycle, explicitly reset transient operation state whose meaning is scoped to the old resource. Do not rely solely on the old resource's close path, event ordering, or an assumption that queued work must already have executed.

**Review rule:** lifecycle tests should include open/close/reopen sequences while deferred work is pending, not just a clean steady-state transition. State that represents "work queued for this object" is as lifecycle-sensitive as cached analysis data.

**Confidence:** Very high. Merged master lifecycle correctness fix authored by John Thacker and accepted by Anders Broman.
