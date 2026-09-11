# Wireshark Dissector Source-File Naming Conventions

This file records durable source-file naming conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Reserve `packet-*` for files that actually implement dissection

Use the `packet-` prefix for source files that contain dissector functionality. When a dissector-support translation unit contains only shared data structures such as `value_string` tables and no packet dissection, name it with the `data-` prefix instead so its role is clear from the tree and build files.

Merged master MR !23332, authored and merged by Michael Mann and reviewed by Jaap Keuter, renamed a large set of data-only files from `packet-*` to `data-*` specifically because they contain no dissection functionality. The MR description states that the naming distinction is intended to make these files easier to identify; its 123-file final change applies the convention repository-wide rather than as a one-off rename.

**Implementation rule:** before naming a new epan/dissector source file, ask whether it registers or implements packet dissection. Use `packet-<name>.*` for actual dissector code and `data-<name>.*` for data-only support modules. Do not let a data-table file acquire a `packet-` name merely because the data is consumed by dissectors.

**Confidence:** Very high. Merged repository-wide naming cleanup authored and merged by a senior maintainer, with explicit purpose and reviewer involvement.
