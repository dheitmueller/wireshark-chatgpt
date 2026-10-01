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

## GUI diagnostics cannot rely on inherited stdout/stderr

Merged master MR !2235 contains direct Guy Harris analysis of terminal-output behavior when Wireshark is launched from a desktop GUI. His tests found materially different destinations across environments. The review distinguishes temporary developer debugging, diagnostics useful for bug reports, dissector-programming failures, and errors a user or site administrator can act on. The point is not to ban diagnostic output, but to ensure it reaches an appropriate retrievable or visible channel.

Merged !2259's introduction of a common ws_debug() path is consistent with that architectural direction: debug policy belongs in common infrastructure rather than in ad hoc print wrappers.

**Logging rule:** do not treat stdout/stderr visibility as a cross-platform contract for GUI applications. Route diagnostics through a common facility according to audience and severity so user-actionable errors are visible, developer diagnostics can be retrieved for reports, and dissector bugs use the packet/dissector reporting path when appropriate.

**Confidence:** Extremely high. Direct Guy Harris cross-platform diagnostic analysis on a merged master change, corroborated by the subsequent merged logging consolidation.
