# Durable conventions from Wireshark MRs 4311–4360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Bound speculative file-format probing

Merged master MR 4344 demonstrates that legal capture-file size is not an appropriate work budget for a weak format sniffer. CAM Inspector and VeriWave probes could otherwise scan huge nonmatching files during open-time type detection, freezing GUI and CLI consumers before a format was even selected. Bound speculative work independently of the maximum file size the committed reader supports. Guy Harris's direct review goes further: the historical 1 GiB cap was still likely far too large and he suggested roughly 1–16 MiB as a more plausible scale. Treat those numbers as review guidance, not a universal fixed constant; the durable rule is to make the budget conservative and justified by discrimination needs.

## Normalize random-access reread semantics at the shared reader boundary

Merged MR 4353 adds a default Lua FileHandler seek+read fallback. Guy Harris points out that ordinary EOF can be normal on a sequential read, but a random-access reread of a packet that was previously read successfully should not silently look like EOF. It should become `WTAP_ERR_SHORT_READ`; where the rule is backend-independent, normalization belongs at the top Wiretap layer so C and Lua readers share the same contract.

## Keep epan below dissectors in the dependency graph

Merged MR 4319 explicitly documents the architecture: code in `epan/` should not depend on `epan/dissectors/`. Dissector code is a client of the epan API, not the reverse. Runtime registration of dissectors, preferences, taps, and related extensions preserves this inversion and supports plugins, potential on-demand loading, and better isolated testing. When common code needs a capability currently implemented by a dissector, introduce an epan-level registration/callback/interface rather than adding an upward hard dependency.

## Namespace project-owned public C API identifiers

Merged MR 4341 renames the getopt compatibility surface from generic system-like names such as `struct option` and `required_argument` to `struct ws_option` and `ws_required_argument`. Public or widely included project APIs should own a predictable namespace so they do not collide with libc, platform SDK, or dependency identifiers.

## Logical deregistration can precede safe reclamation

Merged master MR 4324 is the origin of the dynamic heuristic-registration lifetime rule later corroborated by stable MR 4332 and reviewed MR 4363. Previously dissected packets can retain a pointer to a heuristic-table entry in proto_data. Plugin reload can therefore remove the entry from future lookup immediately, but must defer freeing its backing allocation until the normal redissection/cleanup boundary makes old references unreachable.

## Validate stream framing before entering length-driven TCP reassembly

Merged MR 4312 shows why a TCP dissector cannot always hand the first captured bytes directly to `tcp_dissect_pdus()`: a capture may begin in the middle of an established byte stream, and arbitrary continuation bytes can look like a huge or otherwise plausible length. SMPP first validates fixed-header invariants and known command/status domains; only a plausible PDU boundary enters the length-driven reassembly path. Successful heuristic recognition then binds the conversation. For stream protocols vulnerable to midstream capture, separate boundary recognition from PDU-length extraction.

## Let structural extent determine representation; validate advisory flags separately

Merged MR 4333 changes IS-IS Prefix-SID parsing so sub-TLV length determines whether the body is a label or index, while V/L flags are checked independently and reported malformed when inconsistent. If packet extent already unambiguously determines how many bytes and which representation are present, do not let a corrupt advisory flag suppress those bytes or choose an impossible layout. Decode the structurally supported representation, then diagnose the semantic inconsistency.

## Corroborating rules

Merged MRs 4334 and 4326 reinforce explicit allocator-scope APIs (`pinfo->pool` or another owning scope) over ambient `wmem_packet_scope()`. Merged MR 4315 reinforces reusing the `proto_item *` returned by a standard add-field call when only presentation text must be extended. Merged MR 4314 favors packaging from a release-like source tarball over an unreliable custom build-tree package target. Closed MR 4313 is superseded history and is not implementation precedent.
