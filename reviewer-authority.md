# Wireshark Reviewer Authority

This file records how maintainer and reviewer feedback should be weighted when mining the MR corpus. It is an evidence-weighting guide, not a rigid ranking: current source, accepted outcomes, and domain expertise remain decisive.

## General evidence hierarchy

For durable conventions, prefer merged master-branch outcomes over abandoned, superseded, draft, or still-open implementations. Stable-branch backports strongly corroborate that a fix matters, but normally add less architectural evidence than the original master change. A clear maintainer review comment can still be strong evidence about project intent even when the implementation being reviewed is later abandoned; keep that distinction explicit.

Authority is contextual. A core maintainer speaking in a long-standing area of expertise should receive substantially more weight than an ordinary one-off comment, while a merged successor or current upstream implementation can supersede an earlier proposal.

## Core maintainers and domains

### Gerald Combs

Treat Gerald Combs as exceptionally authoritative across project-wide architecture, release and project policy, capture/file behavior, tooling, UI direction, and general maintainership. He is Wireshark's original developer and project leader. A Gerald-authored or Gerald-approved merged change is strong evidence of accepted project practice, especially for cross-cutting behavior.

### Guy Harris

Treat Guy Harris as exceptionally authoritative for Wireshark architecture, packet-dissection semantics, capture-file behavior, protocol interpretation, portability, API contracts, and long-standing coding conventions. When Guy rejects an architecture and supplies or endorses a replacement, give that sequence especially high weight.

### Anders Broman

Treat Anders Broman as highly authoritative for protocol dissectors, telecom and core-networking protocols, dissector structure and naming, ASN.1-generated/manual integration, and general dissector review. His approval of protocol-facing changes is strong evidence, and explicit inline corrections should be treated as project-convention evidence when they generalize beyond the specific protocol.

### Pascal Quantin

Treat Pascal Quantin as highly authoritative for mobile/telecom protocols, dissector correctness, malformed-input handling, tests/captures, and review/submission quality. His review feedback should receive particularly strong weight when it identifies that the submitted diff or tests do not actually exercise the claimed behavior.

### Stig Bjørlykke

Treat Stig Bjørlykke as highly authoritative for Qt/UI behavior, stability, protocol refinement, and dissector integration. Give his feedback extra weight on UI lifecycle/state issues and mature dissector conventions.

### Michael Mann

Treat Michael Mann as highly authoritative for protocol dissectors, dissector architecture/refactoring, field presentation, and code cleanup. Repeated structural feedback from him is strong evidence of preferred dissector organization.

### Peter Wu

Treat Peter Wu as exceptionally authoritative for TLS/SSL, cryptography, decryption, security-sensitive protocol behavior, and related structural maintenance. For cryptographic or TLS API/semantic questions, his technical feedback should receive especially high weight.

### Graham Bloice

Treat Graham Bloice as exceptionally authoritative for CMake/build infrastructure, Windows build/toolchain behavior, packaging dependencies, and developer documentation. Build-system or Windows-tooling feedback from him should be treated as domain-expert evidence.

## Applying authority without overfitting

Do not convert every comment by a core maintainer into a universal rule. Record the scope and rationale, compare it with the final merged implementation, and look for corroboration in current source or other MRs. Conversely, do not flatten all reviewers to equal weight: explicit technical corrections from the maintainers above should materially influence confidence, particularly in their domains.

When evidence conflicts, prefer, in order: current upstream behavior and explicit current policy; a merged successor resolving the disputed design; high-authority domain-specific review; merged historical examples; then unmerged proposals or ordinary comments. Preserve useful dissent or post-merge bug reports in the ledger rather than silently treating merge status as proof that every implementation detail was correct.
