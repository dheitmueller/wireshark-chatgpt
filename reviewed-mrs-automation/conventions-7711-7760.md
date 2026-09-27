# Durable conventions extracted from !7711-!7760

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Preserve capture evidence and localize repairs

The Guy Harris changes !7754-!7760 distinguish generic capture handling from format-specific correction. Generic code should not silently discard captured bytes merely because reported and captured lengths are inconsistent. When a known producer bug can be reconstructed reliably, correct it in the specific reader that understands the format, as !7756 does for Linux USB mmap captures. Otherwise keep the evidence and expose the inconsistency through expert information.

**Rule:** preserve source evidence by default; apply provable repairs at the narrowest format boundary; diagnose unexplained invariant violations rather than normalizing them silently.

## Hash semantic identity, not raw extensible structures

In merged !7755, new stateless-reset metadata in `quic_cid_t` accidentally affected connection lookup because the hash depended on structure layout. The accepted code hashes only the CID bytes and length, matching the intended equality relation.

**Rule:** hash exactly the fields that define equality. Do not hash an extensible C structure wholesale when it contains padding, caches, or auxiliary metadata.

## Packet-reachable conditions are normal error paths

Merged !7749 replaces a TLS vector bounds assertion with malformed-input reporting and a normal failure result because packet contents can reach that condition. A separate assertion for a true caller/programmer invariant remains.

**Rule:** use assertions only for impossible internal states. Packet-controlled truncation, malformed lengths, preferences, or supported caller conditions require ordinary error, exception, or expert paths.

## Model helper shutdown as asynchronous lifecycle state

Merged !7747 removes an abstraction that treated process shutdown inconsistently across platforms and explicitly points to child watches because exit is asynchronous. Merged !7741 then orders capture shutdown around helper completion: request extcaps to stop, wait for completion callbacks, apply bounded escalation if required, and only then stop the dependent capture child.

**Rule:** a termination request is not an exit event. One lifecycle owner should coordinate request, timeout/escalation, child-exit observation, and dependent teardown, preserving producer/consumer shutdown order.

## Keep dissector-facing APIs discoverable but narrow

Merged !7739 updates the sample dissector as best-practice documentation: use a named dissector so lookup consumers can find it, use automatic preference helpers, prefer ranges when the protocol can use several ports, and do not claim an arbitrary default port without an authoritative assignment.

Merged !7743 reinforces the header boundary: `column-info.h` contains internal column state and routines, while dissectors should use the utility-facing API.

**Rule:** expose deliberate extension points while keeping implementation state internal. Reference/sample code should demonstrate the intended boundary because contributors copy it.

## Compile against the dependency API present at build time

During merged !7728, Pascal Quantin required compile-time guarding for libgcrypt SM3 support and rejected locally defining missing dependency constants or enabling code solely because the runtime library might be newer.

**Rule:** source may only use dependency symbols and constants provided by the compile-time contract. Do not manufacture third-party API definitions locally. If downstream backports make version metadata unreliable, use a narrow capability probe rather than assuming a version number.

## Tree construction cannot control semantic dissection

Merged !7719 removes a tree-presence condition from L2TP zero-length-body recognition. John Thacker states directly that substantive dissection must not depend on whether the tree is present.

**Rule:** null-tree or unreferenced-field fast paths may omit presentation work only. Recognition, state, bounds behavior, diagnostics, tap semantics, and dispatch must remain equivalent.

## Negotiated capability is commonly the intersection of peer state

Merged !7711 fixes MySQL query-attribute parsing so the extended layout is used only when both client and server advertise support. Review confirms that the client may advertise the bit even when the peer cannot use the feature.

**Rule:** for bilateral negotiated features, do not infer wire behavior from one endpoint's advertisement alone. Track effective peer negotiation and keep “unknown because handshake is missing” distinct where partial captures require it.

## Tap refresh requests describe each observable mutation

Merged !7727 makes every newly added Expert Info entry request a redraw. A periodic draw can clear previous redraw state while a long retap is still producing entries.

**Rule:** when a tap callback changes a model in a way that requires refresh, report that refresh requirement for the current mutation rather than relying on an earlier dirty state surviving until completion.

## Down-weight abandoned implementations while retaining review lessons

Closed !7738 demonstrated a false TCP out-of-order classification and was abandoned. Closed !7737 preserves Pascal Quantin's topic-branch submission guidance. Closed !7744 contains useful capture-test and preference-migration review but was replaced. Closed !7722 was explicitly superseded, and !7746 remained a draft.

**Rule:** reviewer objections and process lessons from closed work can be durable evidence, but the abandoned implementation itself is not an accepted architectural precedent.
