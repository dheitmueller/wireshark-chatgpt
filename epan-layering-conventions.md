# Epan and dissector-layer conventions

## Keep protocol-specific shared dissection code in the dissector layer

`epan/` is not a general dumping ground for code shared by more than one dissector. Code belongs in core epan when it implements generic dissection/framework API functionality; common parsing used only by a family of protocols remains part of the dissector layer.

Merged MR !20902, authored by Michael Mann and approved by Anders Broman, moved `epan/xdlc.[ch]` to `epan/dissectors/packet-xdlc.[ch]`. The stated reason was explicit: epan should contain generic dissector API functionality, not "common dissection for a few dissectors." Michael also requested architectural alignment before proceeding with similar moves, and the change was accepted.

For new or refactored shared code, ask what abstraction owns it rather than how many callers it has. Multiple protocol callers alone do not justify promotion into core epan. Prefer a public dissector-layer header when several dissectors legitimately share protocol-family parsing, tables, or helpers.

## Do not make non-dissector consumers link against dissector implementation data

UI, tap, and other non-dissector code should not directly depend on exported `value_string` tables or similar implementation objects owned by individual dissectors. If those consumers need protocol metadata, expose it through an epan-owned lookup/registry interface so the dependency points at the framework abstraction rather than back into dissector implementation code.

Merged MR !20688, authored and merged by Michael Mann, introduced an indirect interface for retrieving dissector `value_string` and `value_string_ext` data. Michael described the architectural goal explicitly: make epan usable as a standalone library and eventually allow Wireshark to be built without linking non-dissector code directly to dissector data; existing UI/tap users of exported dissector tables prevented that separation.

**Architecture rule:** ownership of data and dependency direction matter even for read-only constant tables. A generic UI or tap that needs protocol metadata should ask an epan-level service for it rather than importing a symbol from a protocol dissector. Prefer registries/accessors that preserve the epan↔dissector boundary and permit dissector implementation details to change without creating frontend linkage dependencies.

**Confidence:** Very high. Merged master architectural refactor authored and merged by Michael Mann, with the decoupling/standalone-epan goal stated directly in the MR discussion.