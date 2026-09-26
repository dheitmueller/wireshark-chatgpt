# Notebook updates from !9211 through !9162

Durable additions from this batch:

- Configuration diagnostics: actionable preference-load failures should use the user-visible reporting channel (!9203).
- Filesystem paths: convert local-file URLs before native-path display, and keep main-program-directory identity distinct from current-executable identity (!9183-!9186, !9168/!9179/!9181).
- Parser progress: apply fallback cursor advancement only if the subordinate parser made no progress (!9169/!9177/!9178).
- Field display: resolvable address fields should use normal resolver semantics and preserve resolved/raw custom-column choice (!9201).
- Dynamic registration: packet-driven port registration should claim only endpoint metadata semantically referenced by the target service (!9191).

The batch also corroborates existing notebook guidance on arithmetic width (!9190), typed-item checker modeling (!9182/!9187), representative captures (!9193/!9172), stable-branch scope (!9189), and display-filter operand-order-independent type inference (!9210/!9207).
