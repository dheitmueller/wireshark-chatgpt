# Durable conventions from Wireshark MRs !6411-!6460

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Reassembly identity

Merged master !6440, authored by John Thacker, establishes that frame number alone is not a unique TCP reassembly identity when encapsulation can place multiple PDUs from one stream in one frame. The accepted key includes endpoints, first frame, and the multisegment PDU starting sequence. Hashing may use a cheap discriminator, but equality checks the full key. Temporary lookup keys borrow address storage while stored keys deep-copy it. Merged !6447 then reuses this TCP identity for TLS rather than packing partial sequence bits into an ad-hoc ID.

**Rule:** reassembly equality must represent the full PDU identity, higher layers should reuse transport identity semantics, and key ownership must match key lifetime.

## Finalization-derived results

Merged master !6432, authored by Guy Harris, returns Wiretap reload state from the close operation because that state may only become final during close and the handle is invalid afterward. Stable !6435 and !6438 preserve the semantics. Closed !6430 proposed predicting the value earlier; Guy redirected the design to the close API.

**Rule:** if a result becomes authoritative only during finalization, return it from the finalizer instead of guessing it earlier or inspecting a destroyed object.

## Logical message boundaries for taps

Merged master !6454, with stable !6459 and !6460, fixes HTTP when one buffer contains multiple logical messages. Downstream consumers now receive a subset beginning at the dissector invocation's original offset.

**Rule:** tap and fallback-dissector data must represent the logical object being reported, not an enclosing buffer prefix.

## Qt model/view ownership

Closed !6433 is supporting evidence only, but Roland Knall's review rejects forced reselection as a refresh mechanism when the underlying mutation bypassed the model.

**Rule:** mutate model-owned data through the model and emit its authoritative notifications; do not simulate navigation to force dependent views to refresh.

## Version comparison

Merged !6445 parses dependency versions into integer components and compares tuples instead of converting dotted versions to floating point. Closed !6441 first added a general version library, but Gerald Combs found the dependency broke Windows test startup and suggested the bounded tuple representation.

**Rule:** compare dotted versions structurally, and avoid a broad dependency when a small explicit representation fully models the supported grammar.

## Mutable derived state

Merged !6443, authored by João Valverde, stops reusing cached string representations of syntax-tree nodes because those nodes can mutate and change type. Merged !6444 replaces a fixed pending-jump assumption in code generation with a variable-length representation.

**Rule:** derived caches need reliable invalidation across mutation, and compiler data structures should encode the real cardinality of the language construct.

## Deterministic output parameters

Merged !6415 initializes protocol-tree helper output values even on normal early no-item returns.

**Rule:** every normal return path of an API with an output parameter must leave that output in a defined documented state.

## Review and submission details

Merged !6458 reinforces that new dissectors should include a representative capture, release-note/user-facing registration where appropriate, warning-clean registration functions, and field types chosen for useful filter semantics. Merged !6434 establishes that a redundant long field description should be null rather than duplicate the short name. Merged !6437 preserves ABI diagnostic artifacts even when the check fails. In merged !6428, Guy Harris distinguishes a defensive crash guard from a root-cause fix. During merged !6436, Alexis La Goutte explicitly advised keeping review corrections in the same MR and amending/rebasing and repushing the existing branch.
