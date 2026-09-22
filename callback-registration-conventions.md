# Wireshark Callback Registration Conventions

This file records durable conventions for APIs whose registration step can immediately invoke callbacks or replay previously discovered state. Current upstream source and API documentation remain authoritative.

## Initialize callback-visible state before registration

Do not assume callback registration is a passive operation that only affects future input. Some Wireshark APIs replay objects that were already read or discovered when a callback is registered, so registration itself can synchronously execute consumer code.

Merged master MR !13006, authored by John Thacker, fixes `capinfos` block counting after callback registration replayed blocks that wiretap had already read. The affected counters were reset after registration, erasing counts produced by those synchronous callbacks. The accepted ordering initializes the counting state first and only then installs the callbacks. Merged release-4.2 MR !13007 carries the same fix.

**Implementation rule:** before registering a callback, initialize every object, counter, container, and invariant the callback may observe or mutate. Treat the registration call as a possible call-out into consumer code unless the API explicitly guarantees deferred invocation.

**Review rule:** for callback APIs, check both lifecycle directions: what can run during registration, and what can run during unregistration/cleanup. A seemingly harmless initialization or reset immediately after registration is suspicious if the API can replay existing state.

**Testing rule:** cover the case where relevant objects exist before callback registration, not only the case where all events arrive afterward. This distinguishes replay/synchronous-registration bugs from ordinary event-delivery bugs.

**Confidence:** Very high. Merged master fix by John Thacker with a merged stable-branch backport and a concrete state-loss failure mode.
