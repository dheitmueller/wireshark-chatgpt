# Wireshark Filter Translation Conventions

This file records durable conventions for translating between Wireshark display filters and other filtering or query languages. Current upstream implementation and language specifications remain authoritative.

## Translate semantics, not syntax

A filter translator must only emit an expression when it can preserve the meaning of the source expression in the target language. Similar-looking operators and field names do not imply equivalent semantics: source and target languages can differ in how they treat repeated field occurrences, field-to-field comparisons, ordering, nesting, and stateful primitives.

Merged master MR !15645, authored and merged by Gerald Combs, added the display-filter translation API together with pcap/BPF and Falco translators. John Thacker's review identified several concrete semantic traps. A display-filter field reference such as `ip.src == ${ip.src}` can expand to multiple values, so simple textual substitution is insufficient. A field-to-field comparison such as `ip.src == ip.dst` is meaningful as a display filter but has no straightforward capture-filter equivalent. BPF's `vlan` and `mpls` primitives are order-sensitive, whereas equivalent-looking display-filter expressions can be commutative; even two VLAN IDs can require occurrence-specific semantics in BPF. Gerald responded by refusing at least one unsupported class (a field value on the right-hand side) rather than emitting a plausible but incorrect translation.

**Implementation rule:** make translation capability explicit and conservative. Inspect the source expression's semantic structure, not just its text. Emit a target expression only for constructs whose value/occurrence/order semantics are known to map correctly. If exact translation cannot be guaranteed, report the translator as unavailable or reject that expression instead of silently weakening or changing its meaning.

**Testing rule:** test translators with semantic edge cases, including repeated fields, field references, field-to-field comparisons, nested or order-sensitive protocols such as VLAN/MPLS, and logically reordered source expressions. A successful parse of the generated target string is not enough; the generated expression must select the same intended traffic for the supported subset.

**Confidence:** Very high. Merged architectural feature authored and merged by Gerald Combs, with concrete semantic counterexamples supplied during review by John Thacker and the implementation tightened before merge.

## Construct custom-column filters from typed field occurrences, not display text

Merged **!8725** initially implemented multi-occurrence packet-list filters by splitting the rendered column on commas and wrapping the results in a display-filter membership expression. That does not preserve semantics when a *single* field value contains a comma, a string needs escaping, or a custom column combines different field abbreviations. **!8760** replaces that strategy: col_custom_get_filter() delegates to proto_custom_get_filter(), iterates the relevant field_info instances and uses proto_construct_match_selected_string() to serialize each typed field/value pair. It deduplicates duplicate expressions and joins them with logical OR.

**Rule:** never reverse-parse human-facing custom-column text to infer field identity, occurrence cardinality, quoting or filter literals. Generate each predicate from the actual registered field and stored value; use value-aware literal escaping and maintain each field's identity even for multifield columns. Test comma-containing strings, quoted/escaped text, duplicate occurrences, mixed field types, and first/last occurrence selection.

**Qualified behavior:** the source explicitly leaves **OR vs AND** for multiple occurrences as a design choice, not a proven universally correct interpretation. The !8725 workaround is superseded by !8760; treat the latter as the stronger accepted implementation evidence. !8760 also raised historical minimum GLib to 2.54 for g_ptr_array_find_with_equal_func(), so avoid copying that exact dependency requirement into present-day builds without checking current support. **Confidence:** high, two merged MRs with an explicit correction; no independent runtime tests here.
