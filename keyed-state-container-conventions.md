# Wireshark Keyed State Container Conventions

This file records durable conventions for keyed state containers used by dissectors. Current upstream APIs and implementations remain authoritative.

## Keep keyed state containers type-homogeneous unless the type is explicitly discriminated

A lookup container that returns an untyped pointer still has a logical value type. Inserting two unrelated structure types into the same tree under similar key shapes is unsafe unless every entry carries an explicit discriminator and every consumer checks it before casting.

Merged master MR !15642 fixes a BTLE memory-corruption regression by changing an insertion from `periodic_adv_info_tree` to `connection_parameter_info_tree`. The prior change had put `connection_parameter_info` objects into a tree also used for a different record type, while lookups had no way to distinguish them. John Thacker approved and merged the correction. In the discussion of the superseded/closed refactoring MR !15609, John explicitly asked that this earlier correctness regression be fixed before further restructuring of the dissector.

**Implementation rule:** treat each wmem tree/map/list as having a declared semantic value type even when C exposes the payload as `void *`. Use separate containers for unrelated record types unless a tagged-union/discriminator design is intentional and enforced at every lookup. Similar keys do not make values interchangeable.

**Review rule:** when adding a new insertion path to an existing state container, audit the lookup sites as well as the key construction. Confirm that every producer and consumer agrees on the value type, scope, and lifetime before merging larger refactors on top of the state model.

**Confidence:** Very high. Merged memory-corruption fix with John Thacker approval, plus direct John Thacker review feedback connecting the bug to subsequent refactoring work.