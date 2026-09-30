# Wireshark Dynamic Registration and Resource Lifecycle Conventions

This file records durable lifecycle rules extracted from accepted Wireshark changes. Current upstream source remains authoritative.

## Keep fixed handoff registration separate from preference rebinding

Merged !4400 reorganizes the InfiniBand handoff routine because the routine can be called again when preferences are applied. Stable dependencies, handles, Decode-As entries, heuristic registration, and invariant table bindings are established only on the first handoff. The preference-selected RRoCE UDP binding remains in the repeatable portion, using a handle that persists across calls.

**Implementation rule:** treat a handoff callback as potentially repeatable. Put fixed registrations behind a one-time lifecycle boundary and keep preference-dependent rebinding explicitly repeatable. Do not recreate invariant registrations each time a preference changes.

## Removal from a registry can precede safe storage cleanup

Merged !4363, authored by Stig Bjørlykke, fixes heuristic registration cleanup during Lua plugin reload. Previously dissected packets could still refer to the old registration until redissection completed. The accepted change removes the registration from active use while retaining its storage until the later cleanup boundary.

**Implementation rule:** distinguish logical removal from storage lifetime whenever packet or frame state can retain references. A dynamic registration may stop participating in future lookup before its backing storage is safe to release.

## Follow actual resource ownership after preferences change

Merged !4362, with release-3.4 backport !4361, fixes capture information state when a profile change modifies `capture.show_info` during an active capture. Processing checks whether the session resource actually exists, and teardown closes any resource that was created regardless of the current preference value.

**Implementation rule:** a mutable preference can decide whether a resource is created, but once creation has happened, later use and cleanup must follow actual ownership state. Clear that state when the resource is released so subsequent callbacks cannot mistake it for a live resource.

**Confidence:** High. All three rules come from merged accepted changes; !4363 and !4362 are authored by Stig Bjørlykke and !4361 independently carries the capture-resource correction to a maintained branch.


## Master-origin strengthening: retained packet state drives reclamation timing

Merged master MR 4324 is the origin of the delayed heuristic-registration cleanup later carried to release-3.4 by MR 4332 and independently encountered again in reviewed MR 4363. The UDP path can store a heuristic-table entry pointer in packet protocol data; reloading Lua plugins can deregister that heuristic while already-dissected packets still hold the pointer. The accepted master change therefore removes the entry from active lookup but places its allocation on deferred deregistration cleanup instead of freeing it immediately. Discussion includes a reproducer confirmation that this fixes the crash when combined with Lua reload.

**Strengthened rule:** determine reclamation time from the last possible retained reference, not the registry operation itself. Redissection is often the event that makes packet-attached references obsolete, so plugin, field, and heuristic teardown must respect that boundary.

**Confidence:** Very high. This is the merged master-origin implementation, corroborated by a stable backport and by later merged lifecycle fixes.
