# Wireshark Order-Independent Metadata Conventions

This file records durable conventions for protocols whose logically related metadata can legally arrive in more than one order. Current protocol specifications and upstream source remain authoritative.

## Assemble keyed metadata incrementally when declarations may arrive in either order

Do not make one metadata element's usefulness depend on another element having been seen first unless the protocol specification actually imposes that ordering. If multiple records describe the same logical object, use their stable identity to create or find shared state and merge each record into it as it arrives.

Merged master MR !11392, authored and merged by John Thacker, fixes RTP dynamic-payload handling when SDP `fmtp` appears before `rtpmap`. Previously the dynamic-payload record and its `fmtp` map were created by the `rtpmap` path, so an earlier `fmtp` could not be retained. The accepted implementation creates a placeholder payload record keyed by payload type when `fmtp` arrives first; later `rtpmap` processing finds the same record and fills in the encoding name, sample rate, and channels without discarding the already-collected format parameters.

**Implementation rule:** when sibling metadata records can arrive in arbitrary order, give each path an idempotent find-or-create operation keyed by the protocol's stable identity. Initialize containers once, then merge whichever attributes are currently known into the same object.

**State rule:** placeholders should represent "known object, incomplete metadata," not a fabricated final interpretation. Later declarations must enrich the same object rather than allocate a replacement that loses earlier attributes.

**Review rule:** for any parser that builds state from multiple declaration types, test permutations of their legal ordering. Look especially for code paths that silently do nothing when a prerequisite structure has not yet been allocated.

**Testing rule:** include at least one capture or unit case with the less-common legal ordering, not only the ordering produced by the implementation used during development.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker; the diff directly changes both producer paths to converge on one keyed state object.
