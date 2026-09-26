# Wireshark Column String Conventions

This file records durable conventions for populating packet-list columns from dissectors. Current upstream source remains authoritative.

## Choose column string APIs according to string lifetime and preserve a safe fallback before risky parsing

The column APIs distinguish borrowed constant strings from copied/dynamically produced strings. That ownership distinction matters because a pointer to a temporary buffer cannot safely be retained after the caller returns. Column construction also frequently occurs while dissecting untrusted packet data, where a TVB exception can abort the later formatting path.

Merged master MR !14452 adds Telnet packet summaries to the Info column. During review, John Thacker points out that `col_set_str()` is appropriate for a static `const char *` because it does not copy the string, while dynamically allocated or stack-produced text must use a copying API such as `col_add_str()`/the formatted variants. The accepted implementation leaves a constant `"Telnet Data…"` fallback in the column before parsing, then replaces it with the first successfully produced dynamic summary and appends a bounded number of subsequent entries. The review explicitly notes that this also avoids leaving the Info column blank if dissection throws before a summary is produced.

**Implementation rule:** use the non-copying `col_set_str()` path only when the referenced string has a lifetime that safely outlives the column use, normally a static constant. Use `col_add_str()` or the formatted/copying helpers for stack buffers, allocator-owned temporary strings, and other dynamic values. Do not select APIs solely because their visible output looks equivalent.

**Robustness rule:** if a dissector replaces a generic Info-column description with detail derived from packet parsing, establish the safe generic value first. Replace or append only after the corresponding parse succeeds so an exception or truncation does not leave the column empty or pointing at transient storage. Bound repeated summaries so malformed or unusually dense packets cannot make the column grow without useful limit.

**Confidence:** Very high. Merged master change with direct John Thacker review explaining the ownership/copy semantics and the exception-path motivation; John subsequently approved the revised implementation.
## Route packet-derived column text through the formatting/copying path

Column APIs have a semantic safety distinction in addition to an ownership distinction. A non-copying setter is appropriate for trusted static labels, while text derived from packet data should normally pass through the column path that copies, validates, and formats it for display.

Merged master MR !8616, authored and merged by João Valverde, hardens the column-string helpers so the add/append/prepend paths validate UTF-8 and escape unprintable input through the shared label-string machinery. The same change documents `col_set_str()` as neither formatting nor copying its input and explicitly recommends it for simple static strings such as protocol names, not untrusted or potentially unprintable text. This is early implementation provenance for the ownership guidance later reinforced by !14452.

**Implementation rule:** use `col_set_str()` for constant, already-safe strings whose lifetime is guaranteed. For packet-derived, dynamically constructed, or otherwise untrusted text, use the copying/formatted column APIs so the normal display-sanitization path is applied.

**Review rule:** when a dissector puts text into a packet-list column, review both lifetime and character-domain safety. A pointer can have sufficient lifetime and still be the wrong input for a raw/non-formatting setter.

**Confidence:** Very high. Merged core column-API hardening by João Valverde; later merged review evidence independently reinforces the same API split.
