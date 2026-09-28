# Durable conventions from Wireshark MRs !6461-!6510

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Durable rules

**Unordered formats:** merged !6501 shows that JSON object members must be located by name rather than by position. Extract identifiers needed for later diagnostics in an explicit preparation pass.

**Display-filter evolution:** merged !6488 and !6499 make bitwise AND a typed value-producing expression. New operators must be carried through scanner/grammar, AST, semantic type checks, ftype capabilities, VM execution, diagnostics, docs/release notes, and regression tests, including both operand positions.

**Sibling frontends:** merged !6494 factors common Qt application infrastructure out of the Wireshark-specific layer. Shared lifecycle belongs in a neutral application layer; product-specific policy remains in the product frontend. Configuration identity should be initialized centrally and the legacy/default caller path must remain valid.

**Stream framing:** merged !6493 is cautionary evidence. Syntax changes must not silently replace incremental request processing with EOF-delimited input. Test persistent connections, partial reads, multiple requests, quoted delimiters, and buffer boundaries.

**Qt integer widths:** in merged !6486 Jaap Keuter requested `static_cast`. Keep `qsizetype`/native size values wide through calculations and narrow explicitly only at APIs whose contracts require `int`.

**Incremental builds:** merged !6462 fixes DTD dependencies by naming each source file. Gerald Combs validated the actual transition by modifying an input and rebuilding the dependent target. A clean build alone does not prove dependency correctness.

**Comments:** merged !6465 broadens dumpcap packet-count semantics; Jaap Keuter required the comment to broaden too. Comments should describe the invariant/category implemented by the code, not a stale enumeration.

**Recoverable nonconformance:** merged !6461 decodes a recognizable nonconformant value while surfacing an expert diagnostic. Preserve useful decoded visibility, but follow later/current expert taxonomy such as !6522 for the exact group/severity.

**Evidence weighting:** merged !6509 was subsequently shown still broken and reverted by !6545. A merged-then-reverted patch is negative/superseded evidence; weight the accepted follow-up more heavily.
