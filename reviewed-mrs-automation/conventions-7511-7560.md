# Conventions extracted from !7511-!7560

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Conversation and Follow Stream identity

- A find-or-create helper must create using the same semantic key used for lookup. Merged master !7515 fixes repeated conversation creation when packet_info supplied endpoint or arbitrary conversation elements: lookup honored those elements, while creation had fallen back to src/dst/transport ports.
- When a protocol has already resolved canonical connection identity during dissection, Follow Stream/filter-generation code should reuse that stored state rather than reconstruct identity from packet addresses. Merged master !7530 does this for QUIC because migration and UDP 5-tuple reuse make address/port reconstruction unreliable.

## Reassembly identity

- Reassembly keys must include enough semantic identity to distinguish distinct PDUs without assuming one PDU start per frame or globally unique sequence numbers. Merged !7548 uses first frame plus starting sequence and endpoint identity for TCP multisegment PDUs.
- Reuse shared reassembly-key machinery instead of inventing lossy ad hoc hash mixing. Merged !7551 moves TLS to TCP's reassembly-table functions so equality can use the full multisegment-PDU identity.
- Sender-oriented retransmission analysis and capture-side reassembly state answer different questions. Closed/superseded !7543 reinforces the later merged !7949 lesson; do not simply reinterpret every retransmission as out-of-order data.

## Event-loop and process handling

- If capture callbacks execute on one GLib main-loop thread, do not add a mutex that pretends those callbacks run concurrently. Merged !7558 removes the unnecessary Windows callback mutex.
- Prefer event-loop watches and native bounded wait primitives over periodic process-state polling when the execution model supports them.

## Build tooling

- Prefer documented CMake launcher variables/properties over internal RULE_LAUNCH_* hooks. Merged !7522 uses CMAKE_C_COMPILER_LAUNCHER, CMAKE_CXX_COMPILER_LAUNCHER, and corresponding linker launchers for ccache.
- Setup scripts must tolerate zero positional arguments, especially under set -u. Parse the argument list without directly dereferencing $1; make --help/-h a success path before privilege checks. Merged !7549 is direct evidence.
- Avoid distro-specific workarounds after the underlying package/tool behavior becomes common; simplify to the shared dependency path when support catches up (!7556).

## Field registration and semantic checking

- The existing FT_BOOLEAN rule is about the relationship between mask and display width, not a ban on literal widths. Merged master !7526 and backports !7531/!7532 correctly use FT_BOOLEAN, width 8, mask 0x80 for a one-bit field in an 8-bit container. Maskless FT_BOOLEAN remains BASE_NONE per the typed-item checker convention.
- In typed expression checking, reject an operand whose type cannot support the requested operation before using that operand's type to validate dependent expressions. !7557 prevents FT_NONE arithmetic from reaching the second-term checking path.

## Submission/testing

- Protocol additions should include focused capture evidence that exercises the new behavior. !7525 supplied a MySQL replication pcap plus before/after output.
- Closed exploratory MRs are architecture/review evidence, not implementation precedent. !7543, !7537, and !7516 are explicitly down-weighted in this run.
