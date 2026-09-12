# C Portability Conventions

This file records durable C-language portability conventions extracted from accepted Wireshark changes. Current upstream coding and compiler requirements remain authoritative.

## Use C17-compatible aggregate zero initialization across supported compilers

Wireshark code targeting the project's C17 baseline should use `{0}` when an aggregate needs explicit zero initialization rather than relying on the empty initializer `{}`, which was standardized only in C23 and is not accepted by all supported older compiler versions.

Merged MR !22519 fixes NATS compilation with Visual Studio 2022 17.8 by replacing `tokens[...] = {}` with `tokens[...] = {0}`. John Thacker explicitly requested `{0}`, noting that it works across MSVC versions while the empty form only entered the C standard in C23. He also pointed out that preserving the existing zero-initialization semantics makes the change much easier to review than removing initialization and requiring reviewers to prove it unnecessary.

**Implementation rule:** when equivalent syntax differs in language-version or compiler support, use the form valid under Wireshark's declared language baseline and supported toolchains. For portability fixes, prefer the smallest semantic-preserving change; do not broaden a compiler-compatibility patch into removal of behavior unless that behavior has independently been shown unnecessary.

**Confidence:** Very high. Merged master compiler-portability fix with explicit John Thacker review and approval.
