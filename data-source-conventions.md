# Wireshark Data-Source Conventions

This file records durable conventions for derived tvbuffs and Packet Bytes data sources extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Register a derived tvbuff as a new data source only when it has a distinct underlying data source

`add_new_data_source()` is not a generic declaration that a new tvbuff object exists. Its GUI meaning is that the packet now has another distinct byte representation that should have its own Packet Bytes data-source view. A subset or aligned tvbuff that still shares the same `ds_tvb` as an existing source should not be registered again merely because the tvbuff pointer itself is different.

Merged master MR !21931, authored and merged by John Thacker, changes PER BIT STRING handling so `add_new_data_source()` is called only when `tvb_get_ds_tvb(tvb) != tvb_get_ds_tvb(out_tvb)`. For byte-aligned, whole-byte BIT STRINGs, the aligned tvbuff is only a subset view over the original data source. Registering it produced redundant tabs that did not behave as independent sources; on the motivating frame the change reduced the number of data-source tabs from 4594 to 4027. Release backport !21940 preserves the rule.

**Implementation rule:** decide whether to call `add_new_data_source()` from underlying representation identity, not from whether a helper returned a new tvbuff object. If the derived tvbuff shares `ds_tvb` with an already registered source, normally keep it as a view/subset and do not create another Packet Bytes source.

**Important converse:** when a transformation really creates a distinct representation with its own backing data source—decompression, decryption, bit repacking, text decoding, or a converted ST 291 byte payload—it can be appropriate and necessary to register it. Tree fields backed by that distinct `ds_tvb` need a matching registered source for Packet Bytes selection/highlighting. This refines, rather than contradicts, the local ST 291 finding in `platform-gui-conventions.md`: the packed 10-bit data and converted 8-bit payload are distinct representations, whereas the PER case in !21931 is merely another view of the same underlying source.

**Review check:** when a new data source is added, verify both halves of the contract: (1) its `ds_tvb` is genuinely distinct from an already registered source, and (2) selecting tree fields backed by that `ds_tvb` selects/highlights the intended Packet Bytes view without redundant tabs.

**Confidence:** Very high. Merged master change authored and merged by John Thacker, with an accepted release backport and an explicit GUI/data-source-identity rationale.
