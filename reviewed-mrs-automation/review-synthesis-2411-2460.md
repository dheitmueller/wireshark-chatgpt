# Review synthesis 2411–2460

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs and maintainer-authored changes are weighted more heavily than closed/WIP submissions.

## Public header C/C++ linkage

Guy Harris's merged !2425–!2455 sequence establishes that a header should own the C linkage of the declarations it exposes. Includes should not be pulled wholesale under a caller-supplied C-linkage scope, and feature guards must not make the opening/closing linkage structure differ between build configurations. The no-libpcap follow-up is especially useful evidence because it demonstrates a real feature-disabled compile failure caused by an imbalanced conditional.

Durable lesson: make headers self-contained for C and C++, keep included headers outside broad language-linkage blocks, and validate optional-feature configurations that alter header content.

## pcapng identifier ownership

Guy Harris-authored !2424 explicitly warns that standardized block and option switches may contain only identifiers standardized by pcapng. Private extensions must use the format's extension mechanisms or obtain standardized assignments.

Durable lesson: do not turn externally governed standardized namespaces into Wireshark-private registries.

## Error reporting and write-failure propagation

Guy Harris-authored !2416 introduces a frontend-neutral callback table for capture-file error presentation. !2418 and !2419 then use the common path from Export-PDU, stop processing after a dump failure, and enrich diagnostics with output pathname and frame number.

Durable lesson: common code owns error semantics; frontends own presentation. Propagate the first failed write immediately and report concrete operation context.
