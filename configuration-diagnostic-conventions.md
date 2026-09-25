# Wireshark Configuration Diagnostic Conventions

This file records durable conventions for reporting errors encountered while loading user configuration. Current upstream source remains authoritative.

## Report configuration errors when the invalid configuration is loaded

If Wireshark can determine that a configuration object is invalid while reading it, report the problem at that point with the specific parse/validation cause. Deferring the warning until a user later opens an editor disconnects the diagnostic from the operation that exposed the problem and can replace useful parser context with generic text.

Merged master MR !9889, authored and merged by John Thacker, changes coloring-rule handling so invalid rules are reported when the color-filter file is read, including the rule name, file path, and display-filter compilation error. The old design merely set a preference flag and later displayed a generic warning when the Coloring Rules dialog was opened. Stig Bjørlykke noted that pop-ups during profile switching are generally undesirable, but accepted the early warning here because the user needs to know about the invalid profile content and can fix it immediately.

The accepted implementation also routes the reports through Wireshark's standard warning mechanism, which coalesces multiple warnings rather than producing a separate modal dialog for each bad rule.

**Implementation rule:** surface configuration parsing/validation failures at the read/apply boundary that has the full diagnostic context. Preserve the precise reason and the configuration object involved rather than storing only a boolean that a later UI must turn into a generic message.

**Presentation rule:** use the project's normal reporting/aggregation path so a file containing several bad entries does not create a storm of modal dialogs. Early diagnostics should be actionable without making profile changes or startup unusably noisy.

**Confidence:** Very high. Merged master behavior authored and merged by John Thacker, with explicit UI-timing review and approval from Stig Bjørlykke.
