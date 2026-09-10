# Wireshark State-Modeling Conventions

This file records durable conventions for representing parser and protocol state extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Represent one semantic multi-state domain with one variable, not coupled booleans

When two booleans jointly encode one logical state, they create combinations that are redundant or impossible and force readers and compilers to reconstruct the invariant. Prefer an enum or equivalent single value that directly names every valid state, including an explicit unknown/not-yet-determined state when needed.

Merged MR !25213, authored and merged by Guy Harris, replaces NTS-KE's separate `direction_determined` and `request` variables with one request/response/unknown direction value. Besides making the control flow clearer, the explicit state model avoids a false “may be used uninitialized” warning from older GCC, which could not infer that `request` had necessarily been assigned whenever `direction_determined` was true.

**Implementation rule:** if variables are only meaningful as a tuple and one combination constrains the validity of another, ask whether they are really one finite-state value. Encode that state directly so invalid combinations are unrepresentable and dataflow is evident to both maintainers and compilers.

**Confidence:** Extremely high. Merged master clarity/portability change authored and merged by Guy Harris.

## Reloadable-session state belongs to the session object, not process-static storage

State whose meaning restarts when a capture/session is opened must be represented with that owner and reset at the corresponding lifecycle boundary. A function-local or file-static accumulator can silently carry information from one capture into the next even if it was convenient when only one session was considered.

Merged MR !24543, authored and merged by John Thacker, fixes sharkd cumulative-byte accounting by replacing a static `cum_bytes` variable with `capture_file.cum_bytes` and resetting it in the capture-file open path. The value is a property of the loaded capture, not of the sharkd process lifetime.

**Implementation rule:** classify mutable state by semantic lifetime before choosing storage: packet, conversation, capture/session, or process. If opening/reloading a capture logically starts the value over, put it on the capture/session object and reset it where that object's lifecycle starts rather than relying on static storage.

**Confidence:** Very high. Merged master lifecycle correction authored and merged by John Thacker.

## Acquire a replacement successfully before tearing down the current session resource

Replacing a loaded resource is a state transition, not merely an assignment. When failure to acquire the replacement should leave the current object usable, perform the fallible acquisition first; only after it succeeds should the old resource and its derived state be closed/reset and the replacement installed.

Merged master MR !24462 implements sharkd's `cf_close()` and changes `cf_open()` so the new wiretap handle is opened first. A failed open returns without destroying the existing capture; a successful open then closes/frees the old capture state before installing the new handle. The same lifecycle ordering was accepted in release-4.6 and release-4.4 backports !24464 and !24465. Merged !24473 separately clears cached filter results only after a new file has opened successfully, reinforcing the same commit-point boundary for derived session state.

**Implementation rule:** structure reload/replacement operations transactionally where practical: acquire/validate the new resource, cross the success commit point, then tear down old state and invalidate caches tied to it. Do not destroy the user's current usable state merely because a replacement attempt was made.

**Confidence:** Very high. John Thacker-authored merged master change with two stable backports and a companion merged cache-invalidation fix.
