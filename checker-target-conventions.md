# Wireshark Checker Target Conventions

This file records durable conventions for repository checker and static-analysis build targets. Current upstream build definitions remain authoritative.

## Standalone checker targets must declare the generated sources they inspect

A checker target should be runnable from the build state its name and documentation imply. If it analyzes generated source, the build graph must make generation an explicit dependency of the checker target rather than relying on a developer having performed an unrelated compile first.

Merged master MR !15640, authored, approved, and merged by John Thacker, changes the CMake `CHECKAPI` machinery so the custom target depends directly on `CHECKAPI_SOURCES`. It also passes the actual dissector source list directly instead of writing an auxiliary file list. The stated result is that generated dissector files are present and up to date when CheckAPI runs, allowing the check target to run on a clean source tree rather than only after a normal build. In the discussion of merged !15644, John explicitly points to this independence as enabling code checks to be split into a separate CI job if useful.

**Implementation rule:** encode prerequisites in the build graph of the checker itself. A checker that consumes generated output must depend on the authoritative source-generation target/files. Do not hide an ordering requirement in CI job sequence, developer habit, or an incidental full-build side effect.

**Testing rule:** exercise important standalone checker targets from a clean build tree in CI or periodic validation. This verifies both the checker's logic and its declared dependency graph.

**Confidence:** Very high. Merged master build/tooling change authored and merged by John Thacker, with the clean-tree and CI-decoupling intent stated explicitly.