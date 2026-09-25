# Wireshark MR review findings: !9462-!9511

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Reviewed exactly 50 previously unreviewed MRs, !9511 through !9462. The batch contains 48 merged MRs and two closed/unmerged MRs (!9501 and !9468). Merged work was weighted most strongly.

## Durable findings

- **!9487 + !9490 (John Thacker):** `header_field_info.strings` has type-specific meaning. For `FT_FRAMENUM` it stores `FRAMENUM_TYPE(...)` packet-relation metadata, not a value table. Generic field consumers must check the registered type before interpreting shared metadata members.
- **!9489 (John Thacker):** protocol text should use encoding-aware TVBuff string extraction even when valid packets are expected to contain ASCII. Raw copying bypasses normal malformed-text handling. This was promoted to `text-encoding-conventions.md`.
- **!9474, with backports !9480/!9481 (John Thacker):** a helper used to parse an embedded rejected record must validate its discriminator over the complete wire domain. iSCSI Reject can contain an invalid opcode, so the helper now represents and diagnoses unknown opcodes locally instead of relying on outer-call validation.
- **!9477 (Martin Mathieson):** static-checker exceptions should be narrow and protocol-justified. The accepted change fixes ordinary typed-item findings and adds only the specification-defined STUN non-contiguous method field to the exception set.
- **!9501 (closed):** Alexis La Goutte explicitly states that stable branches backport bug fixes, not refactors/enhancements. The implementation is down-weighted; the policy statement is retained.
- **!9464:** João Valverde explicitly distinguishes the supported MSYS2 MinGW-w64 workflow from toolchain invocations outside MSYS2. Portability review should target the declared support matrix; expanding it is a separate maintenance commitment.

## Corroboration

!9475 reinforces semantic preconditions before third-party decompression; !9486 reinforces first-pass-only state mutation; !9495 reinforces narrow MR scope; !9505 reinforces explicit includes on supported build platforms; !9465 is corrective evidence that shared entry-point context assumptions still need static-analysis scrutiny after merge.

## Frontier

`mr_9461.json` exists, is merged, and was inspected only as a frontier probe. It is not counted as reviewed.
