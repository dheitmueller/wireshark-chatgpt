# Wireshark Extcap Discovery Conventions

This file records durable conventions for extcap/interface discovery, invalidation, and startup behavior. Current upstream source remains authoritative.

## Distinguish 'refresh consumers' from 'invalidate and rediscover expensive providers'

A request to refresh the interface model does not necessarily mean every underlying provider must be discarded and rediscovered. Extcap enumeration can spawn helper processes and is substantially more expensive on some platforms, so invalidation should track whether extcap state itself may have changed.

Merged MR !26302, authored by John Thacker and approved/merged by Gerald Combs, adds an explicit `clearExtcaps` dimension to `InterfaceListManager` refresh requests. Pending refreshes coalesce that requirement, startup paths that have just enumerated extcaps reuse the existing result, and extcaps are cleared only when a scan reason requires rediscovery. The MR reports a typical startup improvement of 500 ms or more on Windows versus roughly 50 ms on UNIX-like systems.

**Implementation rule:** model invalidation separately from notification/refresh. Reuse freshly discovered provider state across adjacent consumers; invalidate it only when the triggering event can make that state stale. When refresh requests coalesce, preserve the strongest pending invalidation requirement rather than letting a cheap refresh erase it.

**Confidence:** Very high. Merged master performance/lifecycle change authored by John Thacker and explicitly approved/merged by Gerald Combs.

## Startup preferences describe when initial discovery is allowed, not merely another change event

A preference such as 'don't load interfaces on startup' is an initialization policy. Treating it as an ordinary boolean delta can accidentally schedule scans while preferences themselves are being initialized or changed for unrelated reasons.

MR !26302 records whether the initial scan has happened and suppresses non-user-initiated discovery while startup loading is disabled. A user-initiated request can still override that startup deferral.

**Implementation rule:** represent initialization state explicitly when a preference has startup-only semantics. Do not infer 'initial versus subsequent' behavior solely from old/new preference values; allow deliberate user actions to cross an initialization deferral when the UI contract requires it.

**Confidence:** Very high. Same Gerald-approved merged InterfaceListManager change, with explicit state and control-flow support.

## Do not recover structured extcap identity by blindly splitting a synthetic display/call name

Extcap bookmark call names combine a real interface name with a bookmark name for user-facing selection, but the underlying interface name is externally supplied and may itself contain whatever separator Wireshark chooses. A delimiter can therefore be useful syntax without being a collision-free identity encoding.

Merged MR !26354, authored and merged by Gerald Combs, initially proposed `::` as the separator. Review pointed out that BLF interface names can contain `::`; Gerald changed the separator to `:` but, more importantly, changed parent recovery to look up the complete synthetic interface in the authoritative extcap registry and follow its explicit `parent_call` relationship instead of splitting on the separator. The tests include bookmark names containing Unicode, quotes, and `/`.

**Implementation rule:** when a synthetic identifier contains externally controlled component names, preserve the component relationship explicitly or resolve it through the authoritative object registry. Do not treat the chosen separator as a guaranteed parse boundary unless the component grammar actually excludes it. Test punctuation, Unicode, and separator-like characters in generated names.

**Confidence:** Extremely high. Merged master design authored/approved/merged by project lead Gerald Combs, with the final approach directly shaped by review of a real separator collision.
