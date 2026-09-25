# Review findings: !9613–!9662

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed the 50 highest-numbered MRs below the prior frontier that were not present in the accumulated review tracking. The batch is 49 merged MRs plus one closed/superseded MR (!9658). Merged work was given greater architectural weight; !9658 is retained mainly because its substantive maintainer review was carried into the later merged MS-DO submission !10124.

## Durable findings promoted

### Decode As direction logic should follow the actual table match

In !9658 John Thacker points out that comparing source/destination ports to a fixed IANA port fails when the same dissector is selected on another port via Decode As. `pinfo->match_uint` identifies the key that actually matched in the transport dissector table. The successor !10124 merged, and upstream MS-DO continues to use this pattern. Promoted to `decode-as-conventions.md`.

### Packet violations belong in Expert Info; impossible implementation states belong on the bug/assertion path

John also requested replacing MS-DO `ws_warning()` calls for unexpected message sizes with reusable Expert Info fields. Tomasz Moń separately identified a branch that packet data alone should never make reachable and requested `DISSECTOR_ASSERT_NOT_REACHED()`. The distinction survives in the merged successor. Promoted to `protocol-input-diagnostics-conventions.md`; the programmer-vs-input assertion distinction already exists elsewhere in the notebook.

### Standards-defined multiplexing should be deterministic once the multiplexed context is known

John Thacker's merged !9638 makes RFC 7983 multiplexing the default RTP policy. The direct RTP path classifies the specified first-byte ranges and calls STUN/TURN, ZRTP, DTLS, or RTP accordingly, while the heuristic RTP entry point declines peer-protocol traffic instead of acting as their router. The source rationale is particularly useful: the STUN heuristic can claim the conversation, TURN ChannelData cannot be heuristically recognized in isolation, and first-pass output can otherwise be wrong. Promoted to `heuristic-dissector-conventions.md`.

### Prefer discovered semantic identity to transport-derived inference

Merged !9654 stores RTPS DomainId learned from participant discovery and keys it by participant GUID for later TCP/UDP packets. Port arithmetic is now only a UDP fallback when discovery has not supplied a DomainId, and the displayed calculated value is marked as potentially inaccurate. Promoted to `protocol-context-fallback-conventions.md`.

### Boolean-selected semantic questions should become named predicates

Merged !9625 replaces `extcap_has_configuration(ifname, is_required)` with `extcap_has_configuration()` and `extcap_requires_configuration()`. The separation makes call sites state whether they are asking about option presence or whether user intervention is actually required, and fixes mandatory arguments that already have defaults. Promoted to `api-mode-parameter-conventions.md`.

### Resource path logic should model packaging layout rather than infer it from the OS

Merged !9621, authored and merged by Gerald Combs, replaces Apple-only bundle assumptions with `ENABLE_APPLICATION_BUNDLE` and moves safe DATA_DIR/PLUGIN_DIR environment overrides ahead of platform/build/bundle/install defaults. Promoted to `filesystem-path-conventions.md`.

## Strong corroborating evidence

- !9653 gives direct John Thacker review of CMake dependency visibility. A link-what-you-use cleanup must still preserve dependencies that are deliberately part of a public target/plugin contract; the accepted change keeps `wsutil` PUBLIC where the API/wmem relationship warrants it and avoids needlessly breaking plugin consumers.
- !9629 is early concrete support for the notebook's narrow-checker-exception rule: known-valid repeated-filter patterns are allowlisted narrowly, while genuine Netlogon/EAP duplicate filter identifiers are fixed rather than hidden.
- !9655 reinforces the generated-source rule. John Thacker identifies a touched dissector as generated from an ASN.1 template and points to the source-side correction rather than accepting a generated-only edit.
- !9622 strongly corroborates `frame-dependency-conventions.md`: dependent frames are mathematically a set, so a hash table avoids quadratic list membership behavior on huge fragment sets. John catches the lazy-initialization NULL iteration edge case before merge.
- !9616 corroborates explicit allocator/context flow: HTTP helpers receive `pinfo`/allocator context instead of calling ambient `wmem_packet_scope()`, and per-packet stats state moves out of a global into packet protocol data.
- !9615 and !9614 independently show Alexis La Goutte requesting representative captures for protocol additions; both contributors respond with concrete capture evidence. This reinforces the existing reproducer convention.
- !9662 adds a native macOS MR build. Its immediate follow-up discussion exposed a missing SpeexDSP setup dependency, illustrating why platform-native CI jobs exercise dependency/bootstrap combinations not necessarily covered by other builds.

## Lower-weight / superseded evidence

!9658 is the only unmerged MR in the batch. It was not rejected for its technical direction; Alexis asked the contributor to stop using the fork's `master` branch and create a fresh MR, which became merged !10124. Its review comments are therefore useful process/API evidence, but the merged successor is the implementation precedent.

Guy Harris appears directly in this batch primarily as the author of documentation cleanup !9613. That change is merged and authoritative for terminology, but it does not add a durable coding/architecture convention comparable to the John Thacker/Gerald Combs findings above.
