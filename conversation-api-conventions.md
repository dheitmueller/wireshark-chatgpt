# Conversation API Conventions

This file records durable conventions for using Wireshark's conversation APIs and attaching dissector state to them. Current `epan/conversation.h` APIs remain authoritative.

## Do not repurpose conversation option parameters as protocol discriminators

The final argument of `conversation_new()` is an options/flags value. A protocol-specific channel, stream, or other discriminator must not be smuggled into that argument merely because it is integer-typed. Use the conversation API variant whose key actually models the intended identity, or attach protocol data through the supported conversation-data mechanisms.

Merged master MR !11676, authored and merged by John Thacker, fixes the IDN dissector after a channel ID had been passed as the `conversation_new()` options argument, producing `DISSECTOR_ASSERT` failures. The accepted change uses `conversation_new_full()`/the full conversation-key mechanism to express the intended identity instead of overloading flags.

**Implementation rule:** interpret API arguments by their declared semantics, not by their C representation. If the protocol needs an extra discriminator, choose an API/key that represents it explicitly rather than storing the value in an unrelated flags/options slot.

## Match allocations to the state lifetime and avoid recreating persistent state on redissection

The same MR also separates file/conversation-lived configuration from per-dissection message state. Persistent configuration is allocated only when first encountered and retained at the appropriate longer-lived scope; transient `message_info` rebuilt on every dissection is allocated from `pinfo->pool`.

**Lifetime rule:** allocate state from the scope that owns it. Do not repeatedly allocate a new file-scope object each time a packet is dissected if the logical object is persistent, and do not use file-scope storage for scratch/message state that is reconstructed on every pass.

**Redissection rule:** persistent state creation must be idempotent across redissection. Check for existing conversation/protocol data before allocating and attaching a replacement object.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, motivated by concrete assertion failures and lifetime misuse.

## Key conversations by the protocol's real correlation invariants

A conversation key should contain the fields that are stable for the protocol relationship being tracked, not every transport-tuple value that happens to be present in one packet. If a peer is permitted to reply from a different source port or another endpoint component can legitimately vary, including that component in the key can split one logical request/response exchange into multiple conversations and break both stateful dissection and Follow Stream behavior.

Merged master MR !11629, authored and merged by John Thacker, fixes SNMP request/response tracking where a fully specified UDP conversation failed when an agent replied from a different ephemeral port. The accepted implementation uses the appropriate wildcard-port conversation semantics so replies remain associated with the request. The same change also fixes the SMUX entry path, which could reach shared SNMP processing without the conversation state expected by that code and trigger an assertion.

**Identity rule:** choose conversation-key fields from the protocol's actual correlation contract. Wildcard transport fields that may validly vary; do not overconstrain identity merely because the first packet supplies a concrete value.

**Initialization rule:** every supported entry path into shared stateful dissection must establish the same required conversation/protocol state before common code consumes it. An alternate encapsulation or handoff path must not rely on initialization having happened through the primary path.

**Review implication:** when request/response correlation is wrong, audit both key specificity and state initialization across all entry points. Symptoms such as unmatched responses, duplicate Follow Stream entries, or assertions can all arise from a conversation key or setup path that does not match the protocol's real topology.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker, with concrete request/response and assertion failures described in the MR.