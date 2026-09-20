# Wireshark Command-Line Option Interaction Conventions

This file records durable command-line option-interaction conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Reject incompatible options instead of silently ignoring them

A command-line option that switches a tool into a distinct operating mode should make the compatibility of the remaining options explicit. If another option cannot affect that mode, silently accepting and ignoring it creates an ambiguous user contract and makes correct behavior look like a bug.

Merged master MR !16392, authored by John Thacker, added `editcap --extract-secrets`. The initial description said the mode would take over output and ignore all other options. Guy Harris explicitly challenged that behavior, asking whether inapplicable options should instead warn or fail so users would not report ignored options as bugs. The accepted implementation tracks whether an editing option was supplied, rejects the combination with `WS_EXIT_INVALID_OPTION`, documents that `--extract-secrets` is incompatible with other options except `-V`, and adds tests for the new extraction behavior.

**Implementation rule:** when a mode-selecting option makes other options semantically inapplicable, define the allowed combinations and validate them after option parsing. Prefer a clear diagnostic and an invalid-option result over silently discarding user intent.

**Documentation/testing rule:** document the compatibility set next to the option and test the mode's behavior, including incompatible combinations when practical. The parser, help text, and manual should describe the same contract.

**Confidence:** Extremely high. Merged master behavior with direct Guy Harris review, accepted by the author and reflected in the final implementation and documentation.
