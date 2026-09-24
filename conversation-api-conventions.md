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