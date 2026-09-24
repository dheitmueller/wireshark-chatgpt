# Wireshark Tap and Export Ownership Conventions

This file records durable ownership and lifetime conventions at dissector/tap/UI and export-object boundaries. Current upstream APIs and source remain authoritative.

## Make ownership transfer explicit when data crosses into a subsystem with a different destructor contract

A pointer that is valid in the dissector's allocator domain is not automatically suitable for a tap or UI object that assumes ownership and later releases it with a different allocator. The handoff boundary must either copy the data into the consumer's ownership domain or use an API whose ownership-transfer contract is explicit.

Merged master MR !11422 fixes a DICOM Export Objects crash. The dissector handed the export-object subsystem a payload allocated from `wmem_file_scope()`, while Export Objects later released `payload_data` with `g_free()` when its window closed. The accepted fix duplicates the payload with `g_memdup2()` so the object being freed by the UI is actually GLib-owned; !11423 carries the same fix to release-4.0.

John Thacker's merged follow-up !11430 sharpens the architecture. The tap callback is made the ownership boundary: it duplicates the strings and payload that the Export Objects entry will own, while the producer-side `dicom_eo_t` fields become `const` borrowed pointers. Since the tap copies them synchronously, the producer's temporary structure and combined payload can use `pinfo->pool` rather than file scope. Merged !11425 independently reinforces the complementary rule that members retained by file-scoped DICOM state should themselves use the matching file scope rather than a separately managed lifetime.

**Implementation rule:** identify the owner on each side of a tap/callback/export boundary. If the consumer will independently retain and free the object, give it storage in the allocator family and lifetime its destructor expects. Keep pre-handoff values borrowed/`const` and in the narrowest valid scope when the consumer makes its own copy.

**Review rule:** when a crash or leak occurs around a tap, export window, callback, or other deferred consumer, trace both lifetime and allocator family across the boundary. Do not solve an ownership mismatch merely by broadening the producer's lifetime; decide where ownership changes and make that transition explicit.

**Confidence:** Extremely high. The rule is supported by a merged master crash fix and stable backport plus an authored-and-merged John Thacker follow-up that deliberately centralizes allocation in the owning Export Objects side and narrows producer state to packet lifetime.
