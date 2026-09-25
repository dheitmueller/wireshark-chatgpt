# Wireshark Capture Metadata Discovery Conventions

This file records durable conventions for capture-file transformations whose metadata can be discovered throughout an input file. Current upstream source remains authoritative.

## Finalize output metadata only after the input phase that discovers it

For capture formats whose interface, name-resolution, decryption-secret, or related metadata can appear after packet records have begun, a transforming tool must not snapshot output parameters before the reader has had a chance to discover that later metadata.

Merged master MR !15138, authored and merged by John Thacker, fixes `reordercap` by moving `wtap_dump_params_init()` until after all frames have been read. Interface Description Blocks and other non-packet blocks may occur in the middle of a pcapng file, so initializing dump parameters immediately after open could omit metadata learned during the scan. The accepted transformation can then emit the collected IDBs/NRBs/DSBs in a valid output position. Release-4.2 MR !15146 carries the same fix.

**Architecture rule:** distinguish file-open metadata from metadata accumulated by parsing. If output construction depends on parser-populated state, define an explicit discovery phase and initialize/finalize output parameters only after that phase is complete.

**Review rule:** test capture transforms with metadata blocks interleaved among packets, not only canonical files with every metadata block at the front. Also identify format structures whose identity is scoped by sections; !15138 explicitly notes that multiple pcapng Section Header Blocks may require additional IDB-number rewriting and therefore remain a separate correctness concern.

**Confidence:** Very high. The rule is supported by a John Thacker-authored and -merged master fix plus an accepted maintained-branch backport.

## Treat pcapng metadata as a stream, not only as an open-time header

pcapng non-packet metadata can appear before the first packet and can also arrive later between packet records. Readers and transforming writers therefore need explicit semantics for both early internal blocks and metadata discovered incrementally while packets are processed.

Merged master MR !9573, authored and merged by John Thacker, changes `pcapng_open()` to consume all initially encountered built-in block types that Wiretap handles internally, including NRBs and DSBs that can appear before an IDB. The code is deliberately conservative for plugin-registered/custom block types: whether those are internal cannot be known reliably without actually reading them, so open-time preconsumption stops rather than guessing ownership.

Merged master MR !9608, also authored and merged by John Thacker, carries the same model into output. Name Resolution Blocks are kept in a growing reader-owned array; dumpers refer to that live array and track how many entries have already been emitted, while mergecap keeps a per-input `nrbs_seen` cursor. That allows streaming transforms to preserve NRBs learned after output initialization instead of snapshotting only the metadata visible at open time.

**Architecture rule:** distinguish built-in internal metadata, externally visible records, and extension-defined blocks whose ownership is not statically known. For metadata that can grow during reading, either delay output metadata finalization until discovery is complete or retain an explicitly live/growing view plus an emitted/seen cursor.

**Review rule:** exercise files with NRB/DSB/IDB metadata before the first packet and interleaved later in the stream. Do not assume a capture's meaningful file-level state is complete after the initial headers.

**Confidence:** Extremely high. Both master changes were authored and merged by John Thacker and form a coherent reader/writer treatment of pcapng metadata discovery.


## Replay retained metadata when redissection recreates downstream consumers

Discovering capture metadata is not enough if the consumer that used it can later be destroyed and recreated. When redissection tears down higher-level state without physically rereading every non-packet record, Wiretap must be able to resupply metadata that was already encountered during the original sequential scan.

Merged master MR !9550, authored by John Thacker, changes pcapng Name Resolution Blocks so their IPv4 and IPv6 mappings are retained as mandatory block data and the already-read NRBs remain attached to the capture reader. Installing fresh name-resolution callbacks then replays those stored blocks. `rescan_packets()` deliberately reinstalls the callbacks during redissection, matching the existing decryption-secrets replay pattern.

**Architecture rule:** capture metadata that contributes to rebuildable dissection state needs both durable storage and replay semantics. A callback that fires only when the block is first read is insufficient if its consumer can be recreated later.

**Review rule:** test redissection after metadata-backed state has been torn down. Verify that name resolution, decryption secrets, and similar non-packet context are restored even when the rescan path rereads packet records rather than every original metadata block.

**Confidence:** Extremely high. Merged master correctness fix authored by John Thacker, and it directly complements the later pcapng streaming-metadata work in !9573 and !9608.
