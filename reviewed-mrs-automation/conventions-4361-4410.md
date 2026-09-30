# Durable conventions from Wireshark MRs !4361–!4410

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Handoff lifecycle

Merged !4400 shows that handoff functions may run again when preferences change. Operations that establish fixed registrations should execute once, while only preference-dependent bindings should be reapplied. Objects needed by later preference updates must remain available across those later calls.

## Delayed lifetime after removal

Merged !4363 shows that removing a dynamic registration and releasing its storage are separate events when already-dissected frames can still refer to the old registration. The old storage must remain valid until the normal redissection or cleanup boundary makes those references obsolete.

## Resource ownership across preference changes

Merged !4362, corroborated by !4361, shows that a preference can change after a session resource has been created. Later processing and cleanup should therefore follow the actual resource state rather than the current preference value, and teardown should clear the stored resource state.

## Nested parsing bookkeeping

Merged !4388 shows that when a child parser consumes framing that the parent would normally process, the child must communicate that fact so the parent does not process the same framing a second time.

## Build and user-interface corroboration

Merged !4384 favors build-target-derived executable paths over reconstructed platform paths. Merged !4381 reports an explicit error when a requested scripting feature is unavailable in that build.

Existing stronger notebook rules are independently corroborated by !4410 (dependency capability detection), !4398 (add-and-return field APIs), !4397 (conservative heuristic defaults), !4394 (sequence diagnostics), !4383 and !4382 (host-side build generators), and !4378 (diagnostic causality). Closed !4391 is lower-weight evidence that a maintenance change must be applicable to the target branch; closed !4364 is not implementation precedent.
