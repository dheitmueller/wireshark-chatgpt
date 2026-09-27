# Wireshark Parser State-Transition Conventions

This file records durable conventions for parser helpers that consume protocol elements while carrying shared semantic state. Current upstream source remains authoritative.

## A parser helper that consumes a semantic unit should own the corresponding state transition

If a public/helper parser accepts the identity of the semantic unit it is consuming and successful parsing changes shared parser state, perform that state transition in the helper rather than requiring every caller to remember a matching assignment. Duplicating the transition at call sites is error-prone, especially when helpers can recurse into nested structures whose private state changes must not become the caller's outer-level state.

Merged master MR !13063 fixes this in the Thrift dissector. The `dissect_thrift_t_*()` helpers accept a `field_id`, but many of them did not update `thrift_opt->previous_field_id`. Callers therefore had to remember to do so manually. Missing updates could fail to detect genuinely decreasing field IDs, while a nested structure could leave its own final inner field ID in the shared state and make a later outer field appear spuriously unordered. The accepted implementation updates `previous_field_id` at the helper boundary after each successfully parsed field; list, set, map, and structure helpers retain the nested result, restore the outer semantic field identity, and then return. Release-4.2 backport !13080 carries the same correction and notes successful fuzzing.

**Implementation rule:** place state advancement at the narrowest API boundary that has both the semantic identity and proof of successful consumption. Callers should not need a parallel bookkeeping assignment merely because they invoked a parser helper.

**Nested-parser rule:** distinguish parent-level parser state from child-private state. After a nested helper returns successfully, restore/commit the state that represents the parent semantic unit before the caller continues. Do not let the final identifier or cursor state of a nested object leak outward unless the API explicitly defines that behavior.

**Review rule:** for parser option/state structures passed through several helper layers, list which fields each helper may read and mutate. Check all success paths, including compact/alternate encodings and nested-container paths, for consistent advancement; a helper family with one missing update can create false expert warnings far from the omission.

**Confidence:** Very high. Merged master correctness fix with an explicit worked failure example in the MR description, maintainer approval by Pascal Quantin, successful pipeline, and a merged stable-branch backport.

## Reset prior-epoch derived state before accumulating the first message of a new epoch

Merged master MR !6813 fixes TLS/DTLS RSA decryption with Extended Master Secret across renegotiation. The previous code reset the old decryption/session state only later while dissecting the Hello, so the second ClientHello could be omitted from the new handshake hash because the old master-secret state was still present when the hash logic ran. The accepted fix moves `ssl_reset_session()` to the handshake boundary before the Hello is added to the new hash and clears prior handshake data when beginning a new client epoch.

**Implementation rule:** when a protocol message starts a new session, handshake, key epoch, or other state epoch, invalidate the prior epoch's derived flags, secrets, hashes, and caches before any logic for the boundary message consults or updates those derived values. Reset-after-update can let stale state suppress or contaminate the first event of the new epoch.

**Testing rule:** exercise at least one transition with retained state such as renegotiation, rekey, or restart, and verify that the boundary message contributes exactly once to the new derived state.

**Confidence:** Very high. Merged master correctness fix from Peter Wu with positive review from John Thacker, Ivan Nardi, and Alexis La Goutte, followed by confirmation from the original reporter before backporting.
