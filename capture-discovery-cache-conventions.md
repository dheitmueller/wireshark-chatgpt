# Wireshark Capture Discovery Cache Conventions

This file records durable conventions for caching capture-interface discovery and other externally obtained runtime capability data. Current upstream capture architecture remains authoritative.

## Cache expensive or privileged discovery only with an explicit invalidation event

Repeatedly asking a helper process to rediscover state can be more than a performance cost: capture discovery may cross privilege boundaries, trigger UAC or other elevation behavior, and duplicate platform work. If the discovered data remains valid until a known refresh/change event, cache it at the application layer and make invalidation part of the architecture rather than relying on callers to remember whether discovery has already happened.

Merged master MR !13624, authored and merged by John Thacker, caches the local capture-interface list in the Qt application after the first `capture_interface_list()` call. Subsequent interface-option parsing can reuse that list instead of spawning additional privileged `dumpcap` queries. Manual “Refresh Local Interfaces” and interface-monitor notifications explicitly clear the cache so the next request performs fresh discovery.

Merged master MR !13641 narrows the same principle for Logray: because Logray only needs extcap interfaces at that point, it calls `append_extcap_interface_list(NULL)` rather than obtaining the full local interface list and invoking `dumpcap`. Gerald Combs noted that avoiding unnecessary privileged-helper behavior is desirable and that a limited implementation is preferable when the frontend does not require the broader capability set.

**Architecture rule:** cache discovery results when their validity boundary is well defined, and centralize both the cached value and its invalidation trigger. A cache without an authoritative invalidation event risks stale capability state; repeated discovery without a reason wastes work and can repeatedly cross privilege boundaries.

**Least-work/least-privilege rule:** request only the discovery domain the caller needs. A frontend that needs extcap interfaces should not implicitly enumerate local capture devices merely because a generic helper can return both.

**Testing rule:** verify first-use discovery, repeated reuse, manual refresh, asynchronous interface-change invalidation, and frontends that intentionally request only a subset of interfaces/capabilities. Count helper invocations where practical so a functional test also detects accidental reintroduction of redundant privileged queries.

**Confidence:** Very high. Two merged master changes by John Thacker establish both explicit cache invalidation and narrowing discovery to the frontend's actual needs; the privilege/helper cost is discussed directly in the accepted work.

## Recognize explicit non-device capture sources before enumerating devices

A user-supplied capture source is not necessarily an interface that should be validated by enumerating the platform's capture-device list. If a source has a platform-defined syntax that identifies another transport class, recognize that class first and bypass unrelated device discovery. This avoids unnecessary helper work and, on privileged capture paths, avoids an unnecessary privilege boundary crossing.

Merged master MR !13602, authored and merged by John Thacker, recognizes Windows named-pipe syntax before retrieving the interface list. A directly specified named pipe is accepted as a pipe without asking `dumpcap` to enumerate capture interfaces. The change also moves the duplicated pipe-name test from `dumpcap` and `rawshark` into a common Win32 helper used by `capture_opts`, `dumpcap`, and `rawshark`, so all capture entry points classify the same name consistently.

**Architecture rule:** classify syntactically explicit special sources before falling back to generic interface discovery. Put platform-specific source-recognition rules in a shared helper when multiple frontends or capture paths must agree on the classification.

**Review rule:** when a caller retrieves an interface list merely to decide whether one explicitly named source is usable, ask whether the source type can be determined directly. Named pipes, extcap endpoints, files, or other special source classes should not be forced through physical-interface discovery without a semantic reason.

**Testing rule:** exercise the special source through every frontend that shares capture option parsing, and verify that using it does not trigger an interface-list query. Also retain ordinary interface-name tests so the fast classification path does not swallow legitimate device names.

**Confidence:** Very high. Merged master correctness/architecture change authored and merged by John Thacker, with explicit consolidation of three formerly duplicated classification paths.