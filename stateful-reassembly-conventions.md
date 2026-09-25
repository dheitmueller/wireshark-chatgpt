# Wireshark Stateful Analysis and Reassembly Conventions

This file records durable conventions for protocol state, sequence analysis, and reassembly extracted from accepted upstream Wireshark changes. Current upstream APIs and dissectors remain authoritative.

## Model the complete protocol identity; use structured keys when a scalar would discard information

State tables and reassembly keys must distinguish every protocol dimension that can legitimately separate simultaneous transactions or streams. Do not compress a larger identity tuple into an undersized integer if doing so drops bits or creates predictable collisions.

Merged MR !26223 fixes MCTP request/response collisions by including the tag-owner bit with the three-bit message tag. Merged MR !26230, authored and merged by Martin Mathieson, independently calls out the same design problem in RLC-NR: a packed reassembly ID can discard UE-ID and sequence-number bits, and Martin explicitly suggests passing separate values into a persistent structured key with a matching equality function instead.

**Implementation rule:** enumerate the protocol identity tuple first. If it does not map collision-free into the API's simple scalar ID, use the API's data/key hooks and a structure with explicit hash/equality semantics rather than lossy bit packing.

**Confidence:** Very high. One merged correctness fix plus explicit architectural follow-up from a long-standing maintainer.

## Reassembly state transitions must remain consumable under loss, reordering, duplicate ACK mechanisms, and retransmission

State machines should be written for the protocol's admissible event ordering rather than assuming the capture begins at the lowest sequence number or that only one acknowledgement mechanism retires data. A valid event that advances protocol state must not leave stale entries at the head of a queue where they can block all future progress.

Merged MR !26211 corrects UDX sequence analysis and reassembly in several related ways: SACK-retired segments are skipped by later cumulative-ACK processing; superseded pending entries are removed so they cannot permanently block reassembly; the lowest observed sequence is tracked independently of the first observed packet; timeout classification takes precedence when the elapsed time meets the RTO; and RTT is attributed to the direction that sent the acknowledged segment. The work was validated with five captures, six focused tests, and comparison against an instrumented libudx implementation.

**Implementation rule:** make state transitions idempotent across overlapping acknowledgement mechanisms; ensure queue/list progress cannot be obstructed by already-retired state; distinguish capture observation order from protocol sequence order; and attribute timing to the endpoint whose transmission is actually being measured. For complex stateful protocols, differential validation against a mature reference implementation is unusually valuable.

**Confidence:** Very high. Merged master fix with broad sequence/reassembly tests and an external behavioral oracle.

## Prefer Wireshark's standard reassembly API unless the protocol genuinely requires different semantics

A hand-built fragment list often recreates only the byte concatenation part of reassembly while missing lifecycle, overflow, duplicate/overlap handling, frame dependencies, generated links, redissection behavior, and UI integration already provided by Wireshark's reassembly infrastructure.

Merged MR !26234 first hardened SPDY's custom reassembly against integer overflow and documented a leaked tvbuff wrapper and other shortcomings. Merged MR !26240, authored by John Thacker and merged by Anders Broman, then replaced the bespoke implementation with a `reassembly_table` and standard fragment APIs. Besides eliminating the custom lifetime problem, the standard implementation automatically marks depended-upon frames and provides normal frame-number linkage. Merged MR !26244 independently notes similar concerns in LBMC's custom fragment list while adding checked length arithmetic.

**Implementation rule:** before maintaining or extending a private fragment accumulator, determine whether Wireshark's standard reassembly API expresses the required protocol semantics. Prefer the standard API when it does; custom reassembly carries responsibility for all overflow, overlap, duplicate, lifetime, dependency, redissection, and presentation behavior that the common framework otherwise supplies.

**Confidence:** Very high. Merged master migration led by John Thacker, preceded and independently corroborated by security-oriented fixes to custom reassembly code.

## Track directional fragment state separately and make redissection independent of traversal order

State used to identify fragments cannot assume that later dissections will revisit packets in the same sequential order as the first pass. Bidirectional protocols also need independent sequence/fragment state in each direction when the same identifiers can advance separately.

Merged MR !25419, authored and merged by John Thacker, fixes COTP TSDU reassembly by moving fragment IDs from global variables into per-flow conversation data, keeping separate IDs for each direction, and retaining enough file-scoped information to recover the correct fragment ID when frames are dissected in an unspecified order on subsequent passes. The motivating case includes multiple reassemblies completing in the same frame, where `fragment_add_seq_next()` otherwise cannot retrieve the right reassembly during redissection.

**Implementation rule:** design reassembly bookkeeping for arbitrary redissection order, not just the first sequential pass. Store any derived identifiers needed to reproduce earlier decisions, and separate state by direction whenever each endpoint can advance its own fragment/sequence namespace.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker, with the redissection and bidirectional failure modes explicitly documented.

## Snapshot sequentially derived wiretap context for random-access reads

Wiretap readers that learn context while scanning a file sequentially cannot assume that the same ambient parser state will be available when Wireshark later requests an individual record by offset. A second-pass or GUI `seek_read` may begin directly at the record body and therefore never revisit the enclosing metadata that established session-specific state during the first pass.

Merged MR !25834 fixes the 3GPP 32.423 nettrace reader after second-pass reads inherited the final session's UE ID and potentially its timestamp. During the sequential read, the accepted implementation records the per-packet session context keyed by the packet's `data_offset`; `seek_read` restores that snapshot before constructing the packet. This makes random access reproduce the context that applied when the record was first encountered rather than relying on mutable last-seen state.

**Implementation rule:** when record interpretation depends on metadata encountered earlier in a sequential file scan, persist the resolved per-record context under a stable record identity such as file offset or record key. Random-access/second-pass reads must restore that context explicitly; do not rely on traversal order or whatever session state a previous read happened to leave behind.

**Confidence:** Very high. Merged master wiretap correctness fix with the first-pass/second-pass failure mode and restoration strategy documented directly in the MR.

## Do not gate reassembly or persistent dissection state on `proto_tree` availability

The protocol tree is presentation state, not an indication that protocol analysis may safely be skipped. Wireshark can dissect without constructing a tree, and first-pass state created in that mode can be required later for redissection, columns, Follow Stream, or dependent frames.

Merged MR !23844, authored by John Thacker, fixes Fibre Channel reassembly that was conditioned on `tree != NULL`. When the first pass ran without a tree, the reassembly table was never populated; later tree-building redissection therefore could not recover the reassembled payload correctly. The accepted fix removes the tree condition while leaving tree-item creation naturally conditional through the normal APIs.

**Implementation rule:** run stateful protocol analysis and reassembly whenever the packet semantics require it, regardless of whether a `proto_tree` is being built. Restrict `tree` checks to presentation work that truly requires tree nodes; do not use them to suppress state transitions whose results must survive into later passes.

**Confidence:** Extremely high. Merged master correctness fix authored by John Thacker, with the tree-less first-pass/redissection failure mode directly documented.

## Include protocol generation/epoch when a sequence namespace can reset

A fragment or message sequence number is only unique within the protocol namespace that defines it. If renegotiation, restart, epoch change, or another generation event can reset that number while older reassembly state still exists, the generation discriminator is part of the reassembly identity.

Merged master MR !20435, authored and merged by John Thacker, fixes DTLS 1.2 handshake reassembly across renegotiation. DTLS 1.2 resets `message_seq` for each handshake while incrementing the epoch, so using `message_seq` alone can collide with an earlier handshake. The accepted code incorporates the epoch into the fragment sequence identifier. The MR explicitly distinguishes DTLS 1.3, where `message_seq` does not reset on key update and the same extra discriminator is not normally needed.

**Implementation rule:** determine the lifetime of every sequence-number namespace. When a protocol generation event resets or reuses the sequence space, include that generation/epoch/session discriminator in the reassembly key rather than treating the raw sequence number as globally unique. Do not add dimensions that the protocol version does not actually use; model the version-specific identity semantics.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with the reset/collision mechanism stated directly.

## Key reassembly by stable protocol/session identity when transport endpoints can migrate

Addresses and ports are convenient default reassembly discriminators only when the protocol guarantees they remain part of a message's identity. Protocols such as QUIC can migrate an established connection to new endpoints while higher-layer data remains in the same logical session, so transport tuples can change in the middle of one fragmented message.

Merged master MR !15380, authored and merged by John Thacker, fixes TLS handshake reassembly across QUIC connection migration. TLS fragments before and after migration belong to the same handshake, but a tuple-derived reassembly key split them into different records. The accepted implementation supplies custom reassembly-table key functions using the stable `SslSession *`, the handshake reassembly ID, and direction. Merged stable-branch MR !15353 independently fixes the complementary QUIC association problem for zero-length connection IDs by attaching an already identified QUIC connection to the newly encountered conversation after migration.

**Implementation rule:** build reassembly identity from protocol dimensions that remain invariant for the lifetime of the fragmented unit. If an established session can survive endpoint rebinding/migration, do not make addresses or ports mandatory parts of the reassembly key; use the protocol session/connection identity plus whatever direction, generation, and message ID dimensions actually distinguish concurrent reassemblies.

**Review rule:** when adding reassembly for a protocol that supports migration, rebinding, tunneling changes, or other endpoint changes, test a fragmented unit that crosses the change. A correct single-tuple capture does not prove that the key models the protocol's true identity.

**Confidence:** Extremely high. The master fix was authored and merged by John Thacker and documents the exact assertion failure caused by endpoint-based fragmentation; the stable QUIC migration fix corroborates the same identity model.

## Treat endpoint-local channel identifiers as directional aliases, not a shared stream ID

Multiplexed protocols may let each endpoint assign its own numeric identifier to the same logical channel. Those identifiers occupy different namespaces and must not be treated as though both peers necessarily chose the same value.

Merged master MR !12503, authored and merged by John Thacker, fixes SSH channel handling by consuming `SSH_MSG_CHANNEL_OPEN_CONFIRMATION` and retaining per-peer maps from sender channel number to recipient channel number, plus recipient-channel mappings to the selected subdissector. The MR explicitly notes that the message establishing the pairing travels in the opposite direction from messages that configure the channel, so lookup must normalize which peer's namespace a number belongs to. It also retains a best-effort path for one-sided captures. Merged master MR !12506, again authored and merged by John Thacker, then builds SSH channel-data reassembly and SFTP dispatch on top of the corrected channel model.

!12503 also records a remaining limitation: once a channel number is closed and later reassigned, a map containing only the latest association is insufficient for arbitrary random packet access. Correct historical redissection needs the identifier's lifetime/generation, or another mapping that can recover the association valid at the packet being dissected.

**Implementation rule:** model each endpoint's identifier namespace separately and build cross-endpoint aliases only from protocol events that establish them. Route persistent state and subdissector lookup through the direction-correct namespace. If identifiers can be reused over the lifetime of a capture, retain enough generation/time/range history that an older packet cannot be resolved through a newer association.

**Confidence:** Extremely high. Both accepted master changes were authored and merged by John Thacker, and !12503 documents the incorrect equal-number assumption, directionality of the mapping event, one-sided-capture concern, and remaining reuse/random-access limitation explicitly.

## Complete reassembly identity can require both transport and protocol dimensions

Replacing a transport-derived reassembly key with protocol fields is not automatically an improvement if either set of fields can collide independently. The key must model the complete identity of simultaneously active fragment streams, which can require combining endpoint/service-instance information with protocol-level message and session identifiers.

Merged master MR !9996 fixes SOME/IP-TP reassembly after an address/port-only key collided for distinct messages. The proposed replacement added SOME/IP service ID, method ID, client ID, session ID, message type, and major version. During review Lars Völker pointed out that protocol fields alone can also collide because distinct service instances may be differentiated only by IP addresses and ports. The accepted structured key therefore includes the transport endpoints/ports together with the SOME/IP identity fields. Anders Broman merged the result.

**Implementation rule:** when a collision exposes an incomplete key, do not simply swap one partial identity model for another. Enumerate all independent namespaces that can distinguish concurrent instances and combine the dimensions required by the protocol and its deployment model.

**Confidence:** Very high. Merged master reassembly correction with direct protocol-maintainer review identifying the missing identity dimensions.
