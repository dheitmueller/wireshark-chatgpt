# Wireshark Input Resource-Limit Conventions

This file records durable conventions for bounding CPU and parser work derived from untrusted capture or protocol data. Current upstream source and protocol specifications remain authoritative.

## Bound attacker-controlled work factors even when the encoded value is syntactically valid

A protocol field may be legal according to its ASN.1 or wire type and still be unsafe to execute literally if it controls an expensive operation. Treat externally supplied iteration counts, decompression expansion, nesting, and similar work multipliers as resource inputs, not merely as integers that passed bounds checking.

Merged release MRs !22594 and !22595, authored by John Thacker and accepted by Michael Mann, cap PKCS#12 password-hash iteration counts at 10,000,000. The ASN.1 permits effectively unbounded values, and the old path could also cast a negative signed value to a huge unsigned count. The accepted fix imposes an implementation sanity ceiling chosen from the protocol/security guidance rather than allowing a capture to request billions of expensive hash iterations.

**Implementation rule:** when packet or file data directly controls potentially expensive repeated work, define a defensible implementation ceiling and reject or decline work above it. Validate signedness before conversion as well; a negative input must not become an enormous unsigned work factor.

**Confidence:** High. Security/resource-exhaustion fix authored by John Thacker and accepted on two maintained release branches, corroborating the notebook's existing hostile-input resource-bounding direction.

## Enforce width-specific variable-length integer limits inside the decoder

A decoder that is told the semantic width of the integer it is producing must enforce the corresponding maximum encoded length itself. Merely stopping at the backing tvbuff boundary is insufficient: an overlong encoding can be structurally invalid for a 16- or 32-bit value even if enough bytes remain to decode it as a 64-bit value.

Merged master MR !22640, authored by John Thacker and approved/merged by Michael Mann, fixes `thrift_get_varint_enc()` so the caller-provided maximum varint length is actually honored instead of always allowing the int64-length encoding. The change resolves an OSS-Fuzz finding and makes the parser contract match the requested integer width.

**Implementation rule:** carry representation limits into the lowest decoder that consumes the variable-length encoding, and stop/reject once that limit is exceeded. Do not rely on later narrowing, range checks, or the outer buffer length to compensate for accepting an overlong representation.

**Confidence:** Very high. Merged OSS-Fuzz-driven master fix authored by John Thacker with the broken helper contract stated explicitly.

## Bound decompressed output independently of recursion depth or container count

Removing an implementation bottleneck does not remove the need for a resource ceiling. A parser can be made iterative instead of recursive, or a container lookup can be made logarithmic instead of linear, while still allowing hostile input to request an unreasonable amount of output memory or decompression work.

Merged master MR !22325, authored by John Thacker, caps HTTP/3 decoded header output at 1 MiB and reports an expert diagnostic when the limit is exceeded. The adjacent merged master MR !22310 improves composite TVBuff scalability by replacing linear child lookup with a `GSequence` and making composite copying iterative, but its description explicitly notes that a separate total-byte limit can still be necessary for compression-bomb inputs.

**Implementation rule:** distinguish structural scalability from resource policy. Optimizing lookup complexity or eliminating recursive stack growth is useful, but parsers of compressed or expansion-capable input should also impose a defensible bound on total produced bytes or equivalent aggregate work.

**Confidence:** Very high. Both are merged master changes authored by John Thacker, and the distinction between implementation scalability and a hostile-input byte ceiling is explicit in the MR descriptions.

## Validate hostile counts before allocation, and apply the limit to every equivalent decode path

A count that directly determines allocation size must be sanity-checked before the allocation occurs; limiting only the later loop is too late to prevent memory amplification. When a protocol has several structurally equivalent count-driven decode paths, the resource limit must be applied consistently to all of them rather than fixing only the first reproducer.

Merged master MR !16237 fixes NFSv4 decoding after a malformed capture could drive `wmem_alloc0_array()` to request roughly 2.2 GiB. The change adds a maximum operation count and performs the check before allocation in all remaining affected NFSv4 array decode paths. The MR includes a small reproducer and a Valgrind trace demonstrating the excessive allocation, making both the resource failure and the effectiveness of the fix directly testable.

**Implementation rule:** validate untrusted element counts before computing or allocating count-sized storage, using a defensible protocol/implementation ceiling. Audit sibling decoders for the same count-to-allocation pattern and apply the limit uniformly. A post-allocation loop bound does not mitigate the allocation itself.

**Testing rule:** for resource-amplification bugs, keep a minimal malformed-input reproducer when practical and use allocation diagnostics, Valgrind, sanitizers, or equivalent tooling to verify that the excessive request occurs before the fix and is prevented afterward.

**Confidence:** Very high. Merged master resource-exhaustion fix with a concrete reproducer and dynamic-analysis evidence, accepted upstream.

## Bound recursive grammar depth and transient parser bookkeeping

Resource limits must cover parser control state as well as final allocations. Recursive type grammars can exhaust the call stack or loop forever during generator analysis, while parser bookkeeping that is logically single-use can accumulate until an otherwise small message exhausts an internal token pool.

The merged PIDL upstream-sync series provides several independent examples. MR !16084 adds an explicit maximum NDR recursion depth (`NDR_RECURSION_CHECK`/`NDR_RECURSION_UNWIND`) after OSS-Fuzz found excessive recursive nesting. MR !16096 detects recursive type-list traversal and fails instead of recursing indefinitely through self-referential structures. MRs !16083 and !16089 change switch, array-size, and array-length bookkeeping tokens to be consumed once their final user has read them; the upstream fixes explicitly describe otherwise reaching the token-list limit and enabling memory/resource denial of service.

**Implementation rule:** identify all attacker-controlled parser-state dimensions: nesting depth, recursion through type graphs, queued tokens, deferred lengths, and similar transient metadata. Give recursive operations an explicit depth or cycle guard, and release/consume one-shot parser state as soon as its semantic lifetime ends rather than retaining it for the rest of the message.

**Testing rule:** fuzzing and malformed-input tests should exercise pathological nesting and repeated bookkeeping constructs, not just oversized byte strings or element counts. A parser that is byte-bounded can still be vulnerable through stack depth or accumulated control state.

**Confidence:** Very high. Multiple merged John Thacker PIDL sync MRs carry upstream fuzz/security rationale, including explicit OSS-Fuzz findings and denial-of-service/resource-limit descriptions.