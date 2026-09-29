# Durable conventions from Wireshark MRs 5411-5460

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master changes are the primary evidence. Stable backports are corroboration. Closed !5415 is discussion history only. Where a later reviewed MR supersedes an older API or policy, the later accepted design remains authoritative.

## Language-standard changes are toolchain-contract changes

MRs !5443, !5452, and !5458 form a coherent accepted C11 transition. Express the global baseline through the build system, document excluded optional features, and reject toolchains below a known unsupported compiler/SDK floor. Earlier merged C17 experiment !5425 is useful negative evidence: a compiler flag alone was insufficient because CMake capability, Windows SDK/preprocessor behavior, macOS deployment targets, and C++ modes surfaced separately.

**Rule:** when raising a language baseline, validate the complete supported toolchain tuple and turn a known-incompatible floor into an early configuration error. Keep narrow compatibility suppressions only for versions known to implement the required semantics.

## Widening shared integer domains requires an ecosystem audit

MR !5459 widened `range_string` limits to 64 bits. Review immediately uncovered 32-bit assumptions in UI structures, loop variables, format strings, comparison helpers, preference code, packet dispatch, and Lua conversion functions.

**Rule:** a type-width migration is not complete when the typedef/struct compiles. Search every reader, writer, formatter, binding, callback, loop index, and interface boundary for narrowing conversions and matching `PRI*`/conversion helpers. Treat new narrowing warnings as evidence of incomplete migration rather than noise to cast away.

## Formatting warnings should drive contract analysis, not reflexive suppression

During !5460, Guy Harris notes that the principled solution to genuinely truncatable formatted output is to allocate a string large enough for the complete result, while also noting that many warnings require inspecting real input bounds and allocator scope.

**Rule:** investigate `-Wformat-truncation` against the actual input/capacity contract. For output that must be complete, prefer length-aware or dynamically sized construction. Do not globally disable the warning merely because a broad mechanical migration exposes difficult cases.

## Logging must preserve stream semantics and reentrancy safety

MR !5435 makes stderr the normal destination for diagnostics so command-line program output remains pipe-safe, while retaining stdout only where an explicit extcap or GUI backward-compatibility contract requires it. MR !5448 adds another logging-specific constraint: code executed while emitting a log message must not call GLib or another path that can itself log.

**Rule:** diagnostics and normal machine-readable/stdout output are separate interfaces. Default logs to stderr for CLI tools unless a documented contract says otherwise, and audit logging callbacks/helpers for recursion into the logging system.

## Half-open text ranges must use a true one-past-end pointer

MR !5449 fixes a timestamp parser call whose end pointer was one byte short because code computed a length from `packet_preamble + 1` but added it to `packet_preamble`.

**Rule:** for APIs taking `[start,end)`, derive the end from the same base object and point exactly one past the final input byte. Avoid mixed-base length expressions.

## Display-filter syntax is a compatibility surface

In !5433, reviewers explicitly avoid removing `~=` because it had already shipped, while adding `!==` and the new `===` operator.

**Rule:** once display-filter syntax ships, preserve accepted spellings through aliases/deprecation where practical. New operator syntax needs parser/VM/tests plus user-facing documentation.

## Custom field formatting is a distinct display mode

MR !5424 removes `BASE_HEX|BASE_CUSTOM`; MR !5419 fixes a crash caused by interpreting `hfinfo->strings` as a value-string table while `BASE_CUSTOM` actually made it custom-format metadata.

**Rule:** treat `BASE_CUSTOM` as a semantic display mode, not an additive numeric-base flag. Code inspecting `hfinfo->strings` must first respect the field's display mode.

## Keep pcapng timestamp metadata internally and serially consistent

MRs !5445 and !5446 are the master origins of the timestamp-resolution invariant previously seen in release-3.6 backports !5466 and !5465.

**Rule:** `tsprecision`, `time_units_per_second`, and serialized IDB resolution describe one semantic quantity and must agree, including for synthetic IDBs.

## Historical changes superseded by stronger later guidance

Merged !5432 introduced `SIZE_MAX` as a magic regex length meaning NUL-terminated input; later merged !5515 replaces that contract with separate semantic APIs. Likewise, spelling cleanups in !5447/!5413 renamed display-filter abbreviations, while later maintainer guidance in !5809/!5772 treats those names as user-facing compatibility surfaces. Preserve the later guidance as authoritative rather than generalizing from older merged mechanics.
