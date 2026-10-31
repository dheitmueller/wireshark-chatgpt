# Wireshark Column String Conventions

This file records durable conventions for populating packet-list columns from dissectors. Current upstream source remains authoritative.

## Choose column string APIs according to string lifetime and preserve a safe fallback before risky parsing

The column APIs distinguish borrowed constant strings from copied/dynamically produced strings. That ownership distinction matters because a pointer to a temporary buffer cannot safely be retained after the caller returns. Column construction also frequently occurs while dissecting untrusted packet data, where a TVB exception can abort the later formatting path.

Merged master MR !14452 adds Telnet packet summaries to the Info column. During review, John Thacker points out that `col_set_str()` is appropriate for a static `const char *` because it does not copy the string, while dynamically allocated or stack-produced text must use a copying API such as `col_add_str()`/the formatted variants. The accepted implementation leaves a constant `"Telnet Data…"` fallback in the column before parsing, then replaces it with the first successfully produced dynamic summary and appends a bounded number of subsequent entries. The review explicitly notes that this also avoids leaving the Info column blank if dissection throws before a summary is produced.

**Implementation rule:** use the non-copying `col_set_str()` path only when the referenced string has a lifetime that safely outlives the column use, normally a static constant. Use `col_add_str()` or the formatted/copying helpers for stack buffers, allocator-owned temporary strings, and other dynamic values. Do not select APIs solely because their visible output looks equivalent.

**Robustness rule:** if a dissector replaces a generic Info-column description with detail derived from packet parsing, establish the safe generic value first. Replace or append only after the corresponding parse succeeds so an exception or truncation does not leave the column empty or pointing at transient storage. Bound repeated summaries so malformed or unusually dense packets cannot make the column grow without useful limit.

**Confidence:** Very high. Merged master change with direct John Thacker review explaining the ownership/copy semantics and the exception-path motivation; John subsequently approved the revised implementation.
## Never end a formatted column in the middle of a UTF-8 code point

Merged **!8697**, authored by **John Thacker**, fixes fixed-size packet-column formatting after vsnprintf() could leave a partial multibyte character. The accepted path checks the formatted size against available capacity, uses ws_utf8_truncate() on codepoint boundaries, and composes prepended/original column portions with ws_label_strcpy() rather than byte-oriented g_strlcat(). Related merged **!8695** initializes the destination terminator in ws_label_strcpy(), including the empty-string/invalid-input cases. Merged **!8688** (João Valverde) uses formatted, display-safe DNS names for Info-column text rather than inserting raw DNS bytes.

**Rule:** test short, exactly-full, overfull and multibyte-at-boundary values for both prepend and append paths. Keep a separate validated display value for columns; raw wire text and filterable protocol data need not be display-safe. **Confidence:** very high, accepted John/João fixes with concrete regressions.
