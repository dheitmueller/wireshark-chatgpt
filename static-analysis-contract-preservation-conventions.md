# Static-Analysis Contract-Preservation Conventions

This file records durable review lessons about fixing compiler and static-analysis diagnostics without changing API or protocol semantics merely to silence the tool. Current upstream source remains authoritative.

## Do not invent a return contract solely to silence a dead-store warning

A warning about an intentionally advanced local cursor does not justify changing a `void` helper into a value-returning API if callers must not use that value. The replacement contract can be more misleading than the warning.

During merged master MR !5920, Jerome-PS explained that the SSH helper's final `offset` advance documents bytes consumed, while its caller intentionally advances using the packet size for recovery from unknown or buggy fields. João Valverde explicitly agreed that an intentional `(void)offset` is preferable to changing the function semantics merely to satisfy the analyzer: using the wrong function semantics is not a better fix than an explicit unused-value marker.

**Rule:** classify the warning first. If the value is intentionally computed for local structure/documentation and no caller may rely on it, suppress or mark that fact narrowly. Add a return value only when the returned quantity has a real, documented meaning that callers may safely consume.

**Confidence:** Very high. Direct João Valverde review on a merged change, with the semantic mismatch explained by the SSH contributor.

## Zero-initialization is a semantic fix only when zero or NULL is valid on every affected path

Merged master MR !5926 fixes PTP uninitialized-variable warnings by initializing scalars to zero and `frame_info` to `NULL`. Dario Lombardo explicitly asked whether those neutral values were valid from a dissection perspective before accepting the approach, noting that the relevant macros already guard a NULL frame pointer.

**Rule:** do not mechanically initialize variables to make analyzers quiet. Verify that the initializer represents a valid neutral state for every path that can observe it. If zero/NULL would change protocol meaning, fix the control flow or establish a different explicit state instead.

**Confidence:** High. Merged warning cleanup with direct reviewer discussion of the semantic validity of the chosen initial values.

## Preserve low-level programmer-error sentinels; fix callers that violate the contract

Merged master MR !5922 began as an attempt to make byte-string conversion tolerate NULL/empty inputs. João Valverde objected to changing the shared `ws_return` behavior because its invalid placeholder exists to expose programming errors rather than turn them into plausible output. Guy Harris proposed making such failures visible as dissector bugs. The accepted direction fixes the dissector call sites so they do not invoke the converter with zero-length data and adds `DISSECTOR_ASSERT(len > 0)` to `tvb_bytes_to_str_punct()`.

**API rule:** when a low-level helper's precondition is a developer contract, do not weaken it merely because a few callers violate it. Repair the callers and keep the failure observable. Packet-controlled malformed input belongs in ordinary validation, while an impossible helper invocation selected by the dissector implementation is a programmer bug.

**Confidence:** Extremely high. Merged core/dissector fix with direct João Valverde and Guy Harris design feedback.
