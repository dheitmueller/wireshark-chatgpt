# Machine Output Framing

Merged master MR !10872, authored and merged by John Thacker, fixes tshark field output by keeping repeated field values separate from the configured aggregator and writing the aggregator only as output framing. This prevents a control-character separator such as tab from being escaped as though it were field data. Stable MR !10875 carries the same fix. Merged follow-up !10877 documents field-value escaping and adds the explicit escape=y|n option while retaining the existing escaped default.

Implementation rule: keep structural separators separate from data until serialization. Apply escaping and quoting to field values, not to framing characters.

CLI rule: when machine-oriented output transforms values, document that transformation and provide an explicit opt-out when raw values are a legitimate use case, while preserving compatibility defaults unless deliberately changed.

Confidence: Very high.
