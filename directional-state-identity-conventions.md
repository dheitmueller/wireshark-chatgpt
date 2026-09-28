# Wireshark Directional State-Identity Conventions

## Direction can be part of protocol-state identity

Merged MR !6378 fixes Bluetooth GATT service discovery when both peers expose services. Request and handle-database keys are extended with direction so state learned from one peer does not collide with state learned from the other.

John Thacker then identifies the subtler case: Mesh Proxy Data In and Mesh Provisioning Data In are duplex connections in which the semantic service direction is opposite the direction of the current packet. The accepted code derives lookup direction from the ATT opcode rather than always using pinfo->p2p_dir verbatim.

**Identity rule:** if two directions can independently own the same numeric handle/request identifier, direction belongs in the state key.

**Semantic-direction rule:** the correct state-key direction is the direction of the protocol object being referenced, which is not always the current packet direction. Derive it from operation semantics where requests, responses, confirmations, or duplex message classes reverse ownership.

**Testing rule:** exercise both peers as state owners and include message classes whose lookup direction is the inverse of packet direction.

**Confidence:** Very high. Merged correctness change with direct John Thacker review that caught a real duplex regression.
