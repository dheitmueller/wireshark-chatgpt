# Wireshark Conversation-Key Normalization Conventions

This file records durable rules for conversation lookup when a dissector derives a logical endpoint tuple that differs from the packet's current `packet_info` tuple. Current upstream source remains authoritative.

## Creation-on-miss must use the same normalized key as lookup

Conversation lookup and conversation creation are two halves of the same identity contract. If lookup first reconstructs, normalizes, wildcards, or otherwise transforms packet endpoints into the protocol's logical conversation key, a lookup miss must create the conversation from that same normalized key. Falling back to a convenience API that uses raw `packet_info` values silently changes the identity being stored.

Merged master MR !11048, authored and merged by John Thacker, fixes this in tunneled EAP conversation handling. Inner protocols can mutate `pinfo` endpoint addresses and ports. The lookup path already reconstructed the intended normalized `addr_1`/`addr_2` key and conversation options, but the miss path could call a generic creation helper based on the altered packet tuple. The accepted fix creates the conversation from the reconstructed key instead. Merged stable backport !11051 carries the same behavior.

**Architecture rule:** normalize once, then use that normalized representation for both lookup and creation. Treat any wildcard flags, direction normalization, logical endpoint substitution, protocol discriminator, or other identity transformation as part of the key contract rather than as lookup-only logic.

**Review rule:** whenever code follows `find(...normalized arguments...)` with a create-on-miss path, compare the complete arguments of the two operations. A generic `find_or_create_conversation(pinfo)` is suspicious if the preceding lookup did not use the raw current packet tuple.

**Testing rule:** exercise encapsulated/tunneled paths where a child dissector or framing layer changes `pinfo` addresses or ports before state lookup. Verify that subsequent packets in both directions recover the same logical state.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker, with an accepted stable-branch backport.