# Wireshark Output Security Conventions

This file records durable security conventions for exporting or rendering capture-controlled data. Current upstream source and user-facing option semantics remain authoritative.

## CSV syntax quoting does not neutralize spreadsheet formulas

Fields exported from a capture remain attacker-controlled even after they have been correctly escaped for CSV syntax. Spreadsheet applications can interpret quoted cells beginning with formula introducers such as `=`, `+`, `-`, or `@` as active formulas, so CSV quoting and spreadsheet-formula neutralization are separate concerns.

Merged MR !26182, authored by Ronnie Sahlberg and merged by Anders Broman, adds `ws_csv_value_is_formula()` plus opt-in formula escaping for both tshark `-T fields` output (`-E escape_formulas=y`) and the GUI CSV export path. Affected values are prefixed with an apostrophe so spreadsheets treat the following content as text. Review discussion clarified that this apostrophe is a spreadsheet control prefix, not an opening quote requiring a matching closing apostrophe.

The option is intentionally disabled by default because formula neutralization changes the exported value and can also affect legitimate negative numbers, which would break existing consumers that rely on Wireshark's historical CSV output.

**Security rule:** when data crosses from packet-controlled text into an execution-capable consumer such as a spreadsheet, apply consumer-specific neutralization in addition to format-level escaping. Do not assume that syntactically valid CSV is inert.

**Compatibility rule:** if secure neutralization changes established machine-readable output semantics, expose the behavior through shared explicit export configuration and preserve the historical default unless the project deliberately chooses a compatibility break.

**Confidence:** Very high. Merged master security feature with CLI and GUI integration, concrete formula-injection examples, and maintainer review of the escaping semantics.
