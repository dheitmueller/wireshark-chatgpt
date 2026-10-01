# ASN.1 semantic context conventions

This file records durable conventions for semantic context that generated ASN.1 dissectors must carry from an enclosing construct into a reusable child decoder.

## Treat inherited semantic selectors as one-shot decode context

A generic ASN.1 child type can represent different protocol semantics depending on the enclosing information element. When the generated child's API does not directly carry that semantic discriminator, packet-private protocol state can bridge the context, but the selector must not become ambient sticky state.

Merged master MRs !2966, !2974, !2967, !2978, !2990, !2998, !2999, and !3010 establish this pattern across RANAP, F1AP, E1AP, S1AP, NGAP, X2AP, HNBAP, and SBC-AP, predominantly authored by John Thacker. Enclosing objects set an `e212_number_type_t`; the generic PLMN identity handler copies that selector to a local and immediately resets the private state to `E212_NONE` before applying the saved value.

Merged !2991 then fixes the key nested case. RANAP defines RAI as LAI plus RAC, so the inner LAI decoder must not replace the more-specific RAI context already established by the outer object.

**Implementation rule:** set inherited semantic context at the narrowest enclosing construct, consume it exactly where the generic child needs it, and reset it immediately unless the protocol explicitly defines a longer lifetime. A nested child must preserve a more-specific outer selector instead of blindly replacing it with a less-specific child value.

**Generated-code rule:** implement the state transfer in the authoritative ASN.1 conformance/template inputs and regenerate the dissector. The accepted series updates source inputs and generated C together.

**Review rule:** include nested-wrapper cases in validation. A simple direct parent→child test can pass while a nested parent→wrapper→child path silently loses the more-specific context.

**Confidence:** Very high. Broad merged master series, mostly authored by John Thacker, plus a concrete follow-up correctness fix for nested RAI/LAI semantics.
