# Wireshark Header-Field Blurb Conventions

## Use a long blurb only when it adds information

Merged MR !6434 added several NetFlow fields. The field checker flagged registrations whose long blurb repeated the short field name verbatim. Martin Mathieson clarified that if there is no additional explanatory text, the blurb should be null.

**Rule:** the header-field blurb is explanatory text, not a required duplicate label. Use a null blurb when the short name already says everything useful.

**Confidence:** High. Merged dissector change with direct reviewer guidance and checker enforcement.
