# Conventions from MRs 4711–4760

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- **Format evolution (MR 4715, Guy Harris):** when a later protocol version assigns meaning to bytes that older writers treated as reserved, historical captures can contain junk there. Prefer authoritative context when available; on ambiguous paths require independent invariants before trusting the new discriminator. Preserve compatibility identifiers used by old captures.
- **Display-filter algebra (MR 4737):** define quantifier semantics for multi-valued relations. If equality is any-equal, ordinary inequality should be its logical complement, all-not-equal. Keep incompatible historical behavior under distinct explicit syntax and update parser, VM, tests, UI diagnostics, docs, and release notes together.
- **Reassembly indexing (MR 4739):** use exact-key maps for metadata owned by one exact frame. Do not use predecessor/range lookup merely because frame numbers are ordered. Test frames that contain several PDUs, split headers, and the tail of one PDU plus the head of another.
- **Stream framing (MR 4750, John Thacker):** when alignment is uncertain, validate message type together with protocol-specific length constraints before committing to a PDU boundary. A framing callback may decline an interpretation rather than manufacture a plausible PDU from continuation or encrypted bytes.
- **Nullable grammar tails (MR 4736):** end-of-message may represent absence only for grammar branches explicitly marked nullable. Missing required data remains malformed. Keep decoded presence state consistent with the semantic result.
- **0/1 storage contracts (MR 4736, Pascal Quantin review):** when a byte-valued output has a numeric 0/1 contract, assign those values explicitly rather than depending on a language boolean or logical-negation representation.
- **Registry source scope (MR 4735):** related registries can describe different namespaces. Absence from a narrower subordinate registry is not proof that an assignment in the parent registry is unassigned; document and use the authority appropriate to each table.
- **Generated-source critical paths (MR 4746, Gerald Combs):** profile build dependencies. If only one compilation unit depends on generated output, isolate it so unrelated compilation can proceed in parallel.
- **Optional build targets (MR 4738):** create and reference optional generated targets under the same capability condition. Prefer removing unnecessary intermediate targets over maintaining conditional placeholders.
- **Partial initialization (MR 4713, Stig Bjørlykke):** initialize owned pointers to known sentinels and make teardown conditional on resources actually initialized/acquired.
