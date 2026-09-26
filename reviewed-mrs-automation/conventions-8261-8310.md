# Durable Conventions Extracted from Wireshark !8261-!8310

This file is the run-level consolidation for the !8261-!8310 review. Topical convention files contain the durable rules that should be reused during future reviews.

1. **Semantic field values are not display labels.** Decode/store the protocol value, and use field display metadata or a returned display string when UI/column text needs escaping or whitespace normalization. Evidence: merged !8290 and !8301; closed !8283 is only precursor evidence.
2. **Persistent state belongs to the protocol scope that owns it.** If sequence state is defined per transport session, use conversation-scoped state; attach packet-specific analysis results to packet proto-data. Include every multiplexing discriminator required by the specification. Evidence: John Thacker's merged !8281.
3. **Bounded formatters must reserve worst-case output expansion plus NUL.** Compute limits in output bytes and flush/grow before consuming another input unit that might exceed the bound. Evidence: Gerald Combs's merged !8302.
4. **Printf-style APIs require a real format string.** Keep ordinary strings and templates distinct; pass ordinary strings through a literal format such as `"%s"`. Evidence: Gerald Combs's merged !8266 and accepted backports !8269-!8271.
5. **Use log domains to separate operational severity from validation strictness.** A diagnostic may be debug-level by default yet selectively fatal under fuzz/CI via a fatal-domain policy. Evidence: João Valverde's merged !8284, !8286, and !8291.
6. **Dissector handles encode caller-context contracts.** If a child entry point expects pseudoheader/private context, call that entry point and supply the documented data rather than invoking a similarly named generic handle. Evidence: merged !8262 and accepted backports.
7. **Reset fuzz-loop selection state per input.** Shell/environment state is persistent unless explicitly cleared; testcase-local range/keep variables must not leak across captures. Evidence: merged !8261.
8. **Public API movement requires matching package ABI metadata.** When a symbol moves between libraries or is newly exported, update symbol manifests for the actual exporting library and correct introduction version. Evidence: merged !8308 and !8277; source-level reminder in !8307.
9. **New dissector functionality should be reviewable with capture evidence and clean static checks.** Representative pcaps plus missing-prototype/typed-field/mask cleanup were part of the accepted !8268 path.
10. **Prefer native structured/scalar keys over stringification and unnecessary heap objects in hot analysis maps.** !8310 reports >10x improvement from a native SEID/address map; !8309 uses direct integer pointer encoding for frame/session maps.
