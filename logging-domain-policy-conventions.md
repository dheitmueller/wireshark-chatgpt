# Wireshark Logging Domain and Provenance Conventions

Merged master MR 3297, authored by João Valverde, performs a broad migration from GLib/private logging paths to the common wslog API. Merged MR 3310 adds log-domain filtering and explicitly records that Critical and Error levels must remain enabled even when ordinary filtering would suppress messages.

Rule: use the shared logging subsystem as the policy boundary for severity and domain filtering. Diagnostics defined as unconditional high-severity failures must not disappear because a normal domain or debug filter is active.

Merged MR 3262 changes ws_debug so function names are supplied centrally by the logging macro. Call sites then remove hand-written prefixes such as ipfix_read and ipfix_open.

Rule: standardized provenance belongs in the common logger when it can derive it correctly. Avoid manually embedding boilerplate function names in message strings because they drift during refactors and produce inconsistent formatting.

Confidence: very high. All cited changes are merged master logging work authored by João Valverde.


## Preserve explicit GLib domain selection

Merged master MR !2280 checks whether `G_MESSAGES_DEBUG` is already set before applying a default and uses GLib's domain-aware default handler on Unix. This preserves selective domain logging instead of replacing it with a broader application default.

**Logging rule:** an application default may fill in absent logging configuration, but should not replace an explicit domain selection.

**Confidence:** Very high. Merged logging behavior change.
