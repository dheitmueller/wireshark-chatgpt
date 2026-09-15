# Wireshark Protocol Compatibility Conventions

This file records durable compatibility conventions learned from upstream merge-request review. Current upstream source and maintainer guidance remain authoritative.

## Protocol and display-filter renames

- Treat protocol short names and display-filter namespaces as user-facing compatibility interfaces. Renaming a protocol or its filter prefix can break saved display filters, coloring rules, profiles, scripts, and automation even when packet dissection itself is unchanged.
- When a protocol is renamed for correctness or expanded scope, preserve the old protocol/filter identity with `proto_register_alias()` where the API supports it. In merged MR !19357 (`pkcs1: rename to pkixalgs`), Pascal Quantin explicitly required an alias because otherwise the rename would break all existing filters; the contributor added the compatibility alias and the MR subsequently merged.
- More generally, when changing registered field names, protocol abbreviations, or other persistent identifiers, explicitly audit compatibility and either preserve aliases where possible or document unavoidable breakage. Correct semantics still take priority, but compatibility impact should be deliberate rather than incidental.

## Heuristic recognition must evolve with the protocol

- Heuristics based on packet shape or length should be revisited when protocol revisions add optional fields or otherwise broaden valid encodings. In merged MR !19345, the NAS EPS detach-request UL/DL heuristic had become too weak after new optional IEs were introduced; the accepted fix additionally checked the mandatory UL mobile-identity length. Pascal Quantin explicitly confirmed that the old heuristic was no longer strong enough and recommended backporting the correction.
- Prefer discriminators tied to mandatory protocol structure over incidental total-length thresholds when both are available. This makes recognition more resilient as optional fields evolve.
