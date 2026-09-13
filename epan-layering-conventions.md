# Epan and dissector-layer conventions

## Keep protocol-specific shared dissection code in the dissector layer

`epan/` is not a general dumping ground for code shared by more than one dissector. Code belongs in core epan when it implements generic dissection/framework API functionality; common parsing used only by a family of protocols remains part of the dissector layer.

Merged MR !20902, authored by Michael Mann and approved by Anders Broman, moved `epan/xdlc.[ch]` to `epan/dissectors/packet-xdlc.[ch]`. The stated reason was explicit: epan should contain generic dissector API functionality, not "common dissection for a few dissectors." Michael also requested architectural alignment before proceeding with similar moves, and the change was accepted.

For new or refactored shared code, ask what abstraction owns it rather than how many callers it has. Multiple protocol callers alone do not justify promotion into core epan. Prefer a public dissector-layer header when several dissectors legitimately share protocol-family parsing, tables, or helpers.
