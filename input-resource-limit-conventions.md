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