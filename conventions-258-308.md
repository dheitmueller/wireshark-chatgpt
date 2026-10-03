# Convention synthesis: Wireshark MRs !258-!308

- Validate packet-derived persistent configuration before storing and before dangerous later use; fuzz state-establishing packets as well as consumers (!300).
- Do not let unrelated conditions suppress a mandatory encapsulated-protocol handoff; audit copied dissector-template baggage (!289).
- Interpret bounded-view offsets in the view's coordinate space and translate to backing storage only at final access (!275).
- Key persistent state by protocol association when one transport conversation multiplexes several independent associations (!272).
- Use `ENC_NA` for uninterpreted `FT_BYTES`, not integer byte order (!277).
- Normalize deliberately tolerated external syntax at API-wrapper boundaries and test canonical plus tolerated forms (!276).
- Keep generator source and generated artifacts synchronized (!296 and stable counterparts).
- Pass the semantic discriminator required by the downstream parser; use returning tree-add APIs when the same field is displayed and consumed (!292).
- Pin cross-implementation derived values with deterministic regression vectors (!281).
