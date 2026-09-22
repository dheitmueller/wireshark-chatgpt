# Wireshark Registration-Handle Initialization Conventions

This file records durable conventions for the initialization and validity tests of protocol, field, subtree, and expert registration state. Current upstream source remains authoritative.

## Treat zero as the unregistered static-state sentinel when the registry assigns positive IDs

For registration handles stored in static duration objects, prefer the subsystem's canonical zero-initialized invalid state instead of maintaining a parallel explicit `-1` convention when zero is already outside the valid registered-ID domain. C guarantees zero initialization for static storage, but adopting that convention safely requires auditing every validity test before changing the declarations.

The merged sequence !13092 → !13091 → !13093 → !13104 performs that migration deliberately. !13092 first prepares dissector validity tests so zero is treated as invalid. !13091 then removes explicit `-1`/initializer values across epan and changes cached protocol-handle logic from equality with `-1` to the semantic contract `<= 0` invalid / `> 0` valid. !13093 applies the same convention to plugins, and !13104 updates tooling/generated initialization so generated and handwritten code agree.

**Migration rule:** change predicates before or together with the sentinel representation. Search for equality checks against the old sentinel, assertions, lazy-cache tests, generators, plugins, and helper macros; the migration is an invariant change across producers and consumers, not a mechanical deletion of initializers.

**Implementation rule:** test a registration handle according to its API domain. Where successful protocol/field IDs are positive, use a positive-validity test rather than relying on a historical spelling of the invalid sentinel. Static declarations can then rely on guaranteed zero initialization without per-variable boilerplate.

**Generation rule:** code generators and conversion tools are part of the convention. A source-tree cleanup is incomplete if regenerated code reintroduces the old representation or plugin templates retain different validity semantics.

Later merged initialization work, including !20611 and its follow-ons already reviewed elsewhere in this notebook, further corroborates zero as the intended static registration initialization model.

**Confidence:** Very high. Four coordinated merged changes cover preparatory predicate auditing, core epan state, plugins, and tools/generated code, with the migration order visible in the accepted series.