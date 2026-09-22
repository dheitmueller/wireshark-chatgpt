# Wireshark Keyed State Container Conventions

This file records durable conventions for keyed state containers used by dissectors. Current upstream APIs and implementations remain authoritative.

## Keep keyed state containers type-homogeneous unless the type is explicitly discriminated

A lookup container that returns an untyped pointer still has a logical value type. Inserting two unrelated structure types into the same tree under similar key shapes is unsafe unless every entry carries an explicit discriminator and every consumer checks it before casting.

Merged master MR !15642 fixes a BTLE memory-corruption regression by changing an insertion from `periodic_adv_info_tree` to `connection_parameter_info_tree`. The prior change had put `connection_parameter_info` objects into a tree also used for a different record type, while lookups had no way to distinguish them. John Thacker approved and merged the correction. In the discussion of the superseded/closed refactoring MR !15609, John explicitly asked that this earlier correctness regression be fixed before further restructuring of the dissector.

**Implementation rule:** treat each wmem tree/map/list as having a declared semantic value type even when C exposes the payload as `void *`. Use separate containers for unrelated record types unless a tagged-union/discriminator design is intentional and enforced at every lookup. Similar keys do not make values interchangeable.

**Review rule:** when adding a new insertion path to an existing state container, audit the lookup sites as well as the key construction. Confirm that every producer and consumer agrees on the value type, scope, and lifetime before merging larger refactors on top of the state model.

**Confidence:** Very high. Merged memory-corruption fix with John Thacker approval, plus direct John Thacker review feedback connecting the bug to subsequent refactoring work.

## Make per-packet protocol-state keys unique across all coexisting data sources

A byte offset is only unique within its coordinate domain. Stateful dissectors that attach multiple logical records to one packet must not key file-scope or packet-scope state solely by an offset that is local to a subset tvbuff, and even a raw offset can collide when the packet contains multiple derived data sources with the same source-relative layout.

Merged master MR !13197, authored and merged by John Thacker, fixes HTTP/3 header-state collisions. It replaces a subset-local offset with `tvb_raw_offset(tvb) + offset`, then also identifies which entry in `pinfo->data_src` owns the tvbuff and uses that data-source index as the `p_get_proto_data()` / `p_add_proto_data()` key. This handles coalesced QUIC packets where multiple decrypted data sources can exist in the same frame and even the same QUIC layer. Merged release-4.2 backport !13201 independently carries the same fix.

**Implementation rule:** define the identity domain of cached protocol state before choosing its key. If multiple same-protocol instances can coexist in a frame, layer, subset hierarchy, or derived data-source list, include every discriminator needed to distinguish those instances. A source-relative offset should normally be paired with a stable source identity when more than one source can occupy the same packet.

**Review rule:** whenever protocol data is keyed by a small integer, offset, or layer number, ask whether two legitimate objects in one packet can share that value. Coalescing, decryption, reassembly, decompression, and nested subset tvbuffs are common ways for apparently unique offsets to collide.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with a merged stable-branch backport and an explicit real-world collision mode involving coalesced QUIC packets.
