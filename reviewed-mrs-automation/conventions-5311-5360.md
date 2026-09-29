# Durable conventions from Wireshark MRs 5311-5360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !5360 through !5311. Merged work is primary evidence; closed !5353, !5322, and !5321 are lower-weight history.

## Learn persistent state independently of tree presentation

Merged !5349 adds DRBD two-phase-commit state. Its accepted path locates conversation data and runs a first-pass `state_reader_fn` under `!PINFO_FD_VISITED(pinfo)` before the `tree == NULL` return. Tree-building happens afterward.

A dissector may run without a protocol tree. State needed by later packets must therefore be learned during the analysis pass that owns it, regardless of whether presentation is requested. Keep first-pass state mutation separate from redissection/tree presentation.

The same MR shares state across the two TCP flows making up one logical DRBD relationship, corroborating the existing rule that conversation identity should follow protocol semantics rather than a convenient individual transport stream.

## Malformed alternatives must terminate useful work

In merged !5352, Pascal Quantin objects to an MBIM invalid-type path that can consume one byte at a time through a large fuzzed input. He requires a return status so the caller breaks the enclosing loop and reports an error.

Strictly positive cursor movement is not always enough: malformed input can still force excessive work when each iteration advances only minimally through an attacker-supplied length. Invalid structural alternatives should terminate at the correct enclosing boundary instead of repeatedly speculating without useful interpretation.

The same review reinforces standard decoding helpers and using the nested field's specified byte order even when it differs from the outer container's normal order.

## Respect CMake's configuration model

Merged !5327 sets a default `CMAKE_BUILD_TYPE` only for single-config generators; with `CMAKE_CONFIGURATION_TYPES`, build type is not treated as the active configuration. Gerald Combs's review exposed a WiX packaging path that made the same wrong assumption. Merged !5344 fixes those paths with `CMAKE_CFG_INTDIR`.

Build and packaging logic must distinguish single-config and multi-config generators. Do not use `CMAKE_BUILD_TYPE` as a universal output-path component.

## Validate baseline-tool capabilities at the baseline

Merged !5319 replaces separate 7-Zip discovery/bootstrap with `cmake -E tar xf`. Gerald Combs explicitly notes that extraction was tested using Wireshark's minimum supported CMake 3.13.

When replacing compatibility/bootstrap code with functionality supplied by an already-required tool, verify the capability against the exact minimum supported version. Once proven, the mandatory tool can reduce extra setup dependencies.

## Keep shared logging semantics shared

Merged !5357 moves extcap from private `--debug` / `--debug-file` controls to standard `--log-level` / `--log-file` behavior. Invalid levels are rejected, and file logging is an additional sink rather than an unrelated private logging mode.

Merged !5317 provides the complementary process-contract lesson: debug/noisy output from a child dumpcap must not be forwarded through the parent's error-message channel. Stream routing can be part of IPC correctness, not merely presentation.

## Keep mechanical and semantic changes reviewable

Closed !5353 is lower-weight implementation evidence, but Jörg Mayer's review is durable process guidance: a huge mechanical WLAN refactor hid semantic fixes in a diff that was difficult to inspect, so he requested those fixes in separate commits.

For very large mechanical changes, isolate behavior-changing fixes so reviewers can distinguish equivalence-preserving churn from changes requiring correctness scrutiny.
