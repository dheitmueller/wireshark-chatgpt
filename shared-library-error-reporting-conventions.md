# Wireshark Shared Error-Reporting Conventions

## Reusable GUI/CLI logic must propagate failure rather than terminate the process

The merged John Thacker sequence !5548, !5549, and !5555 moves text2pcap and ui/text_import onto common command-line/report-message infrastructure. Scanner, conversion, initialization, and write failures are reported through Wireshark's reporting callbacks and returned as explicit import status values. The frontend remains responsible for its final UI or process-exit behavior.

**Implementation rule:** reusable parsing/import code shared by multiple frontends should not terminate the process for ordinary operation failures. Report through the project abstraction, return a typed status, and let the owning frontend decide how that failure affects the application lifecycle.

**Diagnostic rule:** put enough context in the shared layer to identify the failing operation, such as input/output name or packet number, because that layer owns the operation. Frontends should not have to reconstruct low-level failure context.

**Noise rule:** when repeated malformed data would produce the same user-facing warning many times, a one-time prominent warning plus continued log diagnostics can preserve visibility without flooding the UI.

**Confidence:** Very high. Three merged master changes by John Thacker building one coherent shared error path.
