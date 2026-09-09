# Wireshark Qt Message-Dialog Conventions

This file records durable cross-platform message-dialog conventions extracted from accepted upstream Wireshark changes. Current upstream GUI code remains authoritative.

## Use informative text for secondary content that must remain visible across platforms

Qt's message-box fields do not have identical presentation semantics on every desktop. In particular, `detailedText` can be visible as additional text on macOS but concealed behind a “show details” affordance on Windows and X11/Wayland. It is therefore the wrong field for ordinary secondary explanation that users should see without extra interaction.

Merged MR !17619 (`Clean up message boxes.`), authored and merged by Guy Harris, removes the old GTK+-derived primary/secondary delimiter hack and carries primary and secondary strings separately. Secondary explanatory content is placed in the message box's informative-text field instead of detailed text; detailed text remains available for material that is genuinely optional/detail-level.

**GUI rule:** use the message box's primary `text` for the main message and `informativeText` for secondary explanation that should be visible consistently. Use `detailedText` only for genuinely expandable detail whose concealment on some platforms is acceptable. Do not depend on platform-specific rendering of a Qt field to create the semantic hierarchy.

**Confidence:** Extremely high. Merged master GUI cleanup authored and merged by Guy Harris with the cross-platform behavior documented explicitly in the commit description.
