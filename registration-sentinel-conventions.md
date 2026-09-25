# Registration Sentinel Conventions

Merged !10349, authored by Guy Harris, makes protocol ID `-1` explicitly mean “no associated protocol” when registering dissector tables, custom dissector tables, and heuristic lists. Previously this result depended on `find_protocol_by_id()` returning NULL for that value.

**Implementation rule:** when a public API assigns a special value a defined meaning, handle that value explicitly where the API is implemented. This keeps the contract visible and independent of helper implementation details.

**Confidence:** Extremely high. Merged framework change authored by Guy Harris.
