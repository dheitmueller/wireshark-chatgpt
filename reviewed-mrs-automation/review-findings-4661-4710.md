# Review findings — MRs 4661–4710

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Outcome: 45 merged and 5 closed/unmerged. Merged master changes carry the strongest implementation weight; stable backports mainly corroborate their master changes; closed or superseded submissions are retained only as lower-weight history.

## Strong durable findings

- **MR 4677 — TCPCL reassembly identity (Brian Sipos, merged master):** Transfer ID is 64 bits and unique only within a TCP connection. The prior code truncated it to the 32-bit fragment ID and omitted port identity. The merged fix composes the standard address+port key with the full 64-bit ID using custom hash/equality/lifetime callbacks. Preserve the full wire identifier and its true uniqueness scope; extend a generic key instead of truncating it.
- **MR 4679 — display-filter regex abstraction (João Valverde, merged master):** direct `GRegex *` use across parser, VM, and ftypes is replaced by a project-owned `fvalue_regex_t` interface for compile, match, pattern access, and destruction. Parser/VM code depends on regex semantics rather than a third-party representation. This is strong early evidence for isolating dependency types and ownership behind a narrow semantic API.
- **MR 4685 plus 4694/4695 — signedness portability, with Guy Harris:** Raspberry exposed a signed/unsigned comparison between POSIX `st_blksize` and an unsigned limit. Guy explicitly checked that the constant fits signed 32-bit before accepting a cast to `long`, and his follow-up documents the standards/platform assumptions. Casts used to resolve warnings require a type-contract and range proof, not mechanical warning suppression.
- **MR 4686 — pointer/integer portability (Ivan Nardi, merged master):** QUIC Follow Stream code treated pointer-stored list values as 64-bit integers and failed on 32-bit Raspberry. The accepted code uses `GUINT_TO_POINTER` / `GPOINTER_TO_UINT` and a comparator matching the consuming `guint` API. Pointer encoding must honor the conversion macro's width contract.
- **MR 4692 versus closed 4689 — generated-document build graph (Gerald Combs):** a Windows-only shortcut that skipped roff generation was superseded by explicitly representing HTML/manpage generator targets and dependencies so generation can run in parallel. Prefer repairing the build critical path over dropping supported artifacts solely for speed.
