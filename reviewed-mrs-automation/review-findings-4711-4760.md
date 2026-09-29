# Review findings — MRs 4711–4760

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Outcome: 49 merged, 1 closed/unmerged. Merged master changes were weighted most heavily; stable backports mainly corroborate their master changes. The closed proposal, MR 4732, was down-weighted as superseded design history.

## Strong durable findings

- **MR 4715 — SocketCAN CAN FD compatibility (Guy Harris, merged):** a newly defined flag occupies bytes that were reserved in older captures, where those bytes can contain uninitialized junk. The ambiguous capture entry point therefore trusts the FDF bit only when the rest of the flag/reserved-byte pattern is also plausible; entry points with authoritative external context pass an explicit classic-CAN or CAN-FD mode. A historically inaccurate registered dissector name is retained because old captures can refer to it.
- **MR 4737 — display-filter inequality semantics (João Valverde, merged):** for multi-valued operands, `!=` is made the logical complement of `==`: equality uses any-equal while inequality uses all-not-equal. The former existential-not-equal behavior is retained under explicit `~=` / `any_ne` syntax. Parser, VM, tests, release notes, User's Guide, and Qt diagnostics move together.
- **MR 4739 — HTTP/2 streaming reassembly (merged):** reassembly metadata changes from predecessor-style ordered-tree lookup to exact frame-number maps. Each DATA frame participating in a multisegment PDU points directly to that PDU and its exact fragment offset. The regression capture covers multiple complete messages in one DATA frame, split headers, a frame that ends one PDU and begins another, multi-frame large messages, and a 200004-byte message.
- **MR 4750 — BitTorrent framing recognition (John Thacker, merged):** the PDU-length path validates both message type and type-specific plausible length rather than trusting a single apparent type byte. Implausible combinations in continuation/encrypted stream data are not forced through the BitTorrent message parser. CI also caught and prompted correction of a narrowing warning.
- **MR 4736 — CSN1 nullable trailing grammar (merged, Pascal Quantin review):** explicitly nullable nested fields can legally terminate at end-of-message rather than forcing a decode failure. A trailing existence bit with no required content behind it is normalized in decoded state. Pascal also requested explicit numeric 0/1 assignment for a byte-valued representation instead of relying on logical-negation representation.
- **MR 4735 — E.212 registry source scope (John Thacker, merged, Pascal Quantin discussion):** the authoritative MCC assignment list and the narrower MCC+MNC/PLMN list answer different questions. Absence from the subordinate PLMN list must not be turned into an “unassigned” MCC if the authoritative MCC registry still assigns the code.
- **MR 4746 — generated-source build critical path (Gerald Combs, merged):** generated `dissectors.c` is compiled in its own object target so the rest of dissector compilation need not wait for generation. The bottleneck was identified with `ninjatracing`.
- **MR 4738 / MR 4732 — optional documentation targets:** the merged solution removes unnecessary generated-manpage targets and creates `manpages` only when Asciidoctor is available. The competing proposal was closed in favor of this simpler design. This is early corroboration for guarding optional target references with the same feature condition that creates them.
- **MR 4713 — WSLua partial initialization (Stig Bjørlykke, merged):** new Proto objects are zero-initialized and deregistration guards optional members, so cleanup remains valid when construction did not establish every member.
- **MR 4752 — CBOR Sequence wrapper (merged):** RFC 8742 support is implemented as a thin sequence entry point that repeatedly invokes the canonical CBOR item parser and registers the sequence media type, rather than duplicating element decoding.

## Additional review observations

MR 4755 demonstrates master-first packaging correction followed by a requested stable backport. MR 4754 is a direct parser-cursor fix where failing to advance after a count field caused the same byte to be consumed twice. MR 4723's review exposed a bits-versus-bytes ambiguity. MRs 4720/4721 and 4718/4719 show compatibility/performance work around Asciidoctor versions and AsciidoctorJ startup cost. Guy Harris's MRs 4716/4717 are small but authoritative CAN-FD subtree corrections.

No substantive Guy Harris review comment is present in the corpus discussion for MR 4737 despite an explicit request for his opinion; no position is inferred from silence.
