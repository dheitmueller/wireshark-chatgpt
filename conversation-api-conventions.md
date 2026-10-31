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

## Remove a conversation from the hash table that currently owns its key

Merged master !9249, authored and merged by John Thacker, fixes `conversation_set_addr2()`: when the original conversation still has a second port but no second address, its entry must be removed from `conversation_hashtable_no_addr2`, **not** `conversation_hashtable_no_port2`, before the key is updated. The wrong table had been selected for years and could crash because `conversation_remove_from_hashtable()` did not defensively handle a lookup returning NULL. The same correction was merged into release-4.0 (!9254) and release-3.6 (!9256).

**Rule:** when evolving a keyed conversation, derive the removal table from the **current key shape**, not the newly requested endpoint or a similarly named partial-key table. Verify all combinations of missing address/port components, ensure removal occurs before re-insertion, and test that no stale entry or null lookup is left behind. A table-ownership mismatch can turn a valid endpoint update into a segmentation fault.

**Confidence:** Extremely high: John Thacker's merged master fix, explicit failure analysis, and two accepted backports.

## Keep protocol-private state separate from the transport's dispatch conversation

Merged !9123 introduced CONVERSATION_SNMP to avoid SNMP consuming UDP stream identity, but John Thacker's substantive *post-merge* review identified why the implementation was not a complete fix: conversation_set_dissector() on a private SNMP conversation does not determine what the underlying UDP layer dispatches. Applying it to a UDP conversation is itself wrong if packets on that tuple are not all SNMP. He also noted that the generated packet-snmp.c had not been updated through its ASN.1 source, and regeneration in !9252 largely erased the change. He specifically points to !11629 as the duplicate-stream-ID repair and later !19034 as a proposed reintroduction.

**Rule:** allocate independent application state without conflating it with the lower transport conversation's dissector selection. Only bind a lower-level conversation when the transport's future dispatch contract warrants it (heuristic, wildcard ports, or a well-scoped handoff), and update generator input as well as generated output. **Historical status:** merged !9123 is cautionary evidence, *not* a successful final SNMP implementation. **Confidence:** extremely high for John Thacker's explicit corrective analysis.

## Use the intended endpoint-setup API rather than replacing it with generic conversation creation

Closed **!8740** attempted to repair D-Bus after a conversation API rename by replacing conversation_create_endpoint() with conversation_new(). **Simon Holesch** objected that the replacement should be conversation_set_conv_addr_port_endpoints() and distinguished conversation_pt_to_conversation_type() from conversation_pt_to_endpoint_type(). **Anders Broman** said the first attempt merely made compilation work; it was superseded by merged **!8741**, which applies the specific endpoint-setup API.

Merged **!8746** also shows that adding a genuine new transport address/port family (MCTP) can require coordinated updates to address_type, port_type, conversation/endpoint enum mappings, address formatting and Exported PDU port-type TLVs, not just a dissector table registration.

**Rule:** choose conversation APIs by their intended *behavior*—set packet conversation endpoints vs instantiate an independent stored conversation—not by a superficially similar signature. When introducing a transport identity, verify all corresponding endpoint and export mappings. Treat the unmerged !8740 code as a counterexample; rely on Simon's corrected recommendation and merged !8741. **Confidence:** very high for reviewer correction; high for merged MCTP integration.
