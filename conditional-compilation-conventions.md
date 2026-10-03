# Wireshark Conditional Compilation Conventions

This file records durable practices for optional dependencies, version guards, and compile-time feature boundaries.

## Guard feature-only declarations with the same predicate as their users

Conditional compilation creates a semantic region, not just a set of guarded call sites. If all uses of a `static const`, helper table, function, or other declaration disappear when an optional dependency/capability is unavailable, leaving the declaration outside the guard can break strict-warning builds even though the guarded feature itself is absent.

Merged master MR !5 fixes exactly this on configurations using old libgcrypt. Bluetooth Mesh reassembly `fragment_items` objects were used only under a libgcrypt version condition, and QUIC stream-fragment descriptors were used only with `HAVE_LIBGCRYPT_AEAD`; when those consumers were compiled out, the unconditional declarations triggered `-Wunused-const`, which became a build failure under `-Werror`. The accepted change moves each declaration into the same conditional region as its consumers.

**Implementation rule:** when adding or reviewing an optional-feature guard, audit declarations as well as executable statements. Feature-specific static data and helpers should normally live under the same capability/version predicate as every use, unless they intentionally serve an unconditional path.

**Testing rule:** compile representative supported configurations with the optional feature both present and absent, especially under the project's strict warning configuration. A successful feature-enabled build does not prove the feature-disabled translation unit remains warning-clean.

**Confidence:** High. Direct merged master portability fix on a supported older-dependency configuration.
