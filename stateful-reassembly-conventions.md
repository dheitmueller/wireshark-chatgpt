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
