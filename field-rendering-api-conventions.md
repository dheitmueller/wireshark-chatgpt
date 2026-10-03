# Field Rendering API Conventions

## Share registered-field rendering across UI consumers

Field presentation policy belongs with the registered field/type metadata. Packet diagrams, custom columns, tree summaries, and other consumers should not independently recreate base, value-string, string-termination, or display formatting.

Merged master MR !601, authored by Gerald Combs, introduces a shared `proto_item_fill_display_label()` path and moves the Qt packet diagram from direct low-level fvalue formatting to the common field-display logic. Gerald tested custom columns of multiple types and compared tshark output before and after the refactor, reporting identical output.

Closed MR !585 provides authoritative semantic guidance from Guy Harris even though its proposed export did not merge. Guy states that for `FT_STRINGZ`, `FT_STRINGZPAD`, and `FT_STRINGZTRUNC`, the semantic/display string stops before the terminating NUL; padding bytes are not part of the value. He also identifies `proto_tree_add_item_ret_display_string()` as the intended user-facing representation and recommends adding/fetching the field once, then updating surrounding subtree text from the returned value rather than prefetching packet bytes separately.

**Presentation rule:** reuse the core registered-field display path instead of reproducing formatting rules in a frontend.

**Parsing rule:** when the same field value is needed for both the tree and summary text, prefer an add-and-return API and reuse the returned semantic/display value.

**Confidence:** Very high: merged Gerald Combs architecture plus direct Guy Harris API-semantics review.
