# Vendored Generator Conventions

## Keep vendored generator source close to upstream while applying diagnostic policy at the boundary

A vendored build-time generator and the source it generates have different maintenance contracts. Patching the vendored generator only to satisfy local diagnostic policy can create permanent divergence from upstream, while diagnostics in generated Wireshark source can still reveal defects in code that is compiled into the product.

Merged master MR !9253 updates Wireshark's vendored SQLite Lemon sources and moves the exceptional diagnostic handling to the Lemon build target itself. The accompanying README states the intent explicitly: keep lemon.c as close to upstream as possible and patch it only as a last resort. At the same time, diagnostics in Lemon-generated parser source are handled selectively in Wireshark's build rather than inheriting the generator target's exception.

**Implementation rule:** distinguish the vendored generator implementation from its generated product. If a third-party build-time tool is intentionally kept pristine for synchronization and its upstream diagnostic policy differs from Wireshark's, isolate any exception to that tool target instead of carrying a growing local patch stack. Continue to compile and check the generated Wireshark source under the project's normal policy, using narrow generated-code exceptions only where justified.

**Review rule:** a broad diagnostic exception is acceptable only when its scope matches the intentionally vendored tool boundary. Do not let a convenience exception on the generator leak into generated dissector/parser code that executes in Wireshark.

**Confidence:** Very high. Merged master maintenance change with the upstream-synchronization and diagnostic-boundary rationale documented directly in the MR and Lemon README.
