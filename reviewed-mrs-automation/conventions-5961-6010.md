# Conventions 5961-6010
- Use frame proto data for frame-local facts and correctly keyed persistent state for cross-frame facts; do not use one global for interleavable relationships.
- Caller-defined table keys need caller-defined destruction semantics.
- When an optional dependency is absent, compile out its implementation and reject or disable impossible CLI and GUI choices.
- Checkers should match real source files and registrations, not filename or symbol substrings.
- When a standard contradicts itself, triangulate independent structural evidence and captures.
- Treat heuristic dissector display names as user-facing semantic labels, not merely transport labels.
- Keep field type, consumed width, mask, and semantic value width consistent; padding is not part of a semantic field just because it shares an enclosing layout.
