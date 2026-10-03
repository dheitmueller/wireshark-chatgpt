# Conventions from Wireshark MRs !960-!1009

This file records durable rules extracted from the !960-!1009 review batch. Current upstream source remains authoritative.

## Follow the real protocol layering instead of reimplementing a parent transport

Merged master MR !1001 makes OBD-II a subdissector of ISO 15765-2/ISO-TP rather than keeping partial transport/reassembly behavior inside OBD-II. Guy Harris explicitly verified that ISO 15765-2 is the transport/network layer referenced by the OBD-II specification.

**Architecture rule:** when framing, segmentation, or reassembly belongs to an existing parent protocol, compose the child beneath that parent and reuse the parent's mature state/reassembly machinery. Do not duplicate a partial transport implementation inside an upper-layer dissector.

**Confidence:** Very high. Merged master change with direct Guy Harris protocol-layering review.

## Name a dissector for the wire protocol, not the application that happens to use it

During review of merged master MR !970, Jaap Keuter rejected naming the new dissector after Mosh because Wireshark dissects the protocol, not the application. The accepted implementation uses SSyncP; because the obvious `ssp` abbreviation was already occupied, it chooses a distinct protocol abbreviation rather than falling back to an application name.

**Naming rule:** protocol/file/filter identity should describe the wire protocol. If the natural abbreviation collides, disambiguate the protocol name rather than substituting a product/application label.

**Confidence:** Very high. Direct Jaap Keuter review incorporated before merge.

## One-bit boolean state should be tested for truth, not equality to TRUE/FALSE

In merged master MR !968, Peter Wu noted that one-bit boolean bitfields can have a true numeric representation other than literal 1, for example -1 for a signed one-bit field. The accepted code therefore uses `x` and `!x` rather than `x == TRUE` and `x == FALSE`.

**C rule:** use truth-value tests for boolean/bitfield state unless the value is actually an enum/status domain with a named comparison contract. Do not assume every true representation numerically equals 1.

**Confidence:** Very high. Direct Peter Wu review incorporated into the merged QUIC change.

## Propagate semantic state through the context actually consumed downstream

Merged master MR !984 fixes LE Coded PHY dissection by copying the decoded PHY from the Nordic capture-specific context into the common Bluetooth context used by the downstream dissector. Without that propagation the next layer omitted the coding indicator and parsed later bytes at the wrong offset. Stable backport !986 carries the same fix.

**Context rule:** once a parent or capture-specific layer derives state needed by a shared subdissector, place it explicitly in the common context before calling downstream code. Correct private state is useless if the consumer reads a different context object.

**Confidence:** Very high. Merged master correctness fix plus maintained-branch backport.

## Initialize every shared context member on every producer path

OSS-Fuzz issue 25007 led to merged master MR !961, which initializes Bluetooth ACL context pointers that BTLE producer paths had left unset before passing the context downstream. Gerald Combs noted that zero-initialized wmem allocation could also reduce this class of omission.

**Context/memory rule:** treat a dissector-data structure as an interface contract. Every producer must initialize every member a consumer can inspect. Prefer zero-initialized allocation when zero is a valid default/sentinel, while still setting nonzero semantic defaults explicitly.

**Testing rule:** fuzzing is especially valuable for finding producer paths that leave shared context partially initialized.

**Confidence:** High. Merged fuzz-derived fix; the zero-allocation suggestion is maintainer guidance rather than a mandatory accepted implementation choice.

## Optional-feature guards must cover feature-only helpers as well as call sites

Merged master MR !974 fixes a QUIC build without `HAVE_LIBGCRYPT_AEAD`. Ivan Nardi caught that helper definitions used only by AEAD code were outside the capability guard; Pascal Quantin moved the guard boundary so the helper dependency graph and its consumers are disabled together.

**Build rule:** feature-off configurations are supported configurations. Guard feature-only state/helpers and all their uses consistently, and compile/test with the feature disabled rather than relying only on the default build.

**Confidence:** Very high. Review correction incorporated before merge.

## Recognition paths must prove their minimum captured length before reading discriminators

Merged master MR !980 changes RPC-over-RDMA probing to require `MIN_RPCRDMA_HDR_SZ` before reading fields used to determine whether the packet is RPCoRDMA. The source comment explicitly states that the `tvb_get_ntohl()` probe should not throw while deciding whether the packet belongs to the protocol.

**Dissector rule:** a heuristic/recognition path must first prove that all bytes needed for its discriminator are captured. Truncated or unrelated traffic should be declined cleanly, not rejected by an exception caused by the probe itself.

**Confidence:** High. Merged master robustness fix.

## Use add-and-return proto-tree APIs when code also needs the displayed field value

During review of merged master MR !979, Anders Broman requested replacing separate fetch-plus-display sequences with `proto_tree_add_item_ret[u]int()` where applicable.

**API rule:** when a field must both be added to the tree and consumed programmatically, prefer the matching typed `proto_tree_add_item_ret_*()` helper. It keeps extraction, bounds/encoding handling, and displayed field provenance on one API path.

**Confidence:** High. Direct Anders Broman review incorporated into the merged R-GOOSE change.

## Schema/configuration-driven dissectors need reproducible fixture material in tests

Merged master MR !999 adds Protobuf/gRPC tests together with packet captures, the protobuf schema trees needed to interpret them, and Lua integration fixtures. The tests use tshark-visible assertions and can be selected directly with pytest.

**Testing rule:** if dissection depends on external schemas/configuration, commit or otherwise provide the minimal deterministic fixture material required by the test. A capture alone is not a complete regression vector when decode semantics depend on side inputs.

**Confidence:** High. Merged feature-test suite with multiple captures and schema fixtures.

## Protocol-tree field masks should be checked against the actual packet layout

Merged master MR !960 fixes several RFC 2190 masks; the author states that the packet diagram made the errors obvious. Stable backports !969 and !976 carry the same correction.

**Review rule:** for bitfields, cross-check registered masks and widths against the specification's packet diagram/layout, not only against existing code or value tables. This is an independent semantic check alongside the typed-item checker.

**Confidence:** High. Merged master fix plus two maintained-branch backports.
