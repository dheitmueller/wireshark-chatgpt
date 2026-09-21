# Wireshark Capture Metadata Discovery Conventions

This file records durable conventions for capture-file transformations whose metadata can be discovered throughout an input file. Current upstream source remains authoritative.

## Finalize output metadata only after the input phase that discovers it

For capture formats whose interface, name-resolution, decryption-secret, or related metadata can appear after packet records have begun, a transforming tool must not snapshot output parameters before the reader has had a chance to discover that later metadata.

Merged master MR !15138, authored and merged by John Thacker, fixes `reordercap` by moving `wtap_dump_params_init()` until after all frames have been read. Interface Description Blocks and other non-packet blocks may occur in the middle of a pcapng file, so initializing dump parameters immediately after open could omit metadata learned during the scan. The accepted transformation can then emit the collected IDBs/NRBs/DSBs in a valid output position. Release-4.2 MR !15146 carries the same fix.

**Architecture rule:** distinguish file-open metadata from metadata accumulated by parsing. If output construction depends on parser-populated state, define an explicit discovery phase and initialize/finalize output parameters only after that phase is complete.

**Review rule:** test capture transforms with metadata blocks interleaved among packets, not only canonical files with every metadata block at the front. Also identify format structures whose identity is scoped by sections; !15138 explicitly notes that multiple pcapng Section Header Blocks may require additional IDB-number rewriting and therefore remain a separate correctness concern.

**Confidence:** Very high. The rule is supported by a John Thacker-authored and -merged master fix plus an accepted maintained-branch backport.
