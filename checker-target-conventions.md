# Wireshark Checker Target Conventions

This file records durable conventions for repository checker and static-analysis build targets. Current upstream build definitions remain authoritative.

## Standalone checker targets must declare the generated sources they inspect

A checker target should be runnable from the build state its name and documentation imply. If it analyzes generated source, the build graph must make generation an explicit dependency of the checker target rather than relying on a developer having performed an unrelated compile first.

Merged master MR !15640, authored, approved, and merged by John Thacker, changes the CMake `CHECKAPI` machinery so the custom target depends directly on `CHECKAPI_SOURCES`. It also passes the actual dissector source list directly instead of writing an auxiliary file list. The stated result is that generated dissector files are present and up to date when CheckAPI runs, allowing the check target to run on a clean source tree rather than only after a normal build. In the discussion of merged !15644, John explicitly points to this independence as enabling code checks to be split into a separate CI job if useful.

**Implementation rule:** encode prerequisites in the build graph of the checker itself. A checker that consumes generated output must depend on the authoritative source-generation target/files. Do not hide an ordering requirement in CI job sequence, developer habit, or an incidental full-build side effect.

**Testing rule:** exercise important standalone checker targets from a clean build tree in CI or periodic validation. This verifies both the checker's logic and its declared dependency graph.

**Confidence:** Very high. Merged master build/tooling change authored and merged by John Thacker, with the clean-tree and CI-decoupling intent stated explicitly.

## Compile-test public installed headers from a downstream-style consumer

A header being accepted while building Wireshark itself does not prove that it is self-contained and usable through the installed public include layout. Packaging validation should compile representative public headers from a small external-consumer translation unit so missing public dependencies, include-order assumptions, or packaging omissions are detected before downstream users encounter them.

Merged master MR !15265 extends Debian's `headers-check.c` to include `<epan/dfilter/dfilter.h>`, specifically to avoid regressions such as Debian bug #1068410. The check is intentionally a tiny consumer-style compile rather than a normal source-tree build, so it exercises whether the installed development headers expose everything required to compile against them.

**Implementation rule:** when adding or changing a public development header, include it in the repository/package's external-header compile check where one exists. The test should use the same public include form a downstream application would use, not private source-tree paths that can mask missing dependencies.

**Review rule:** distinguish "the library itself builds" from "the installed SDK surface builds for consumers." Header/API packaging changes need both properties validated.

**Confidence:** High. Merged master packaging fix tied to a concrete downstream Debian failure and accepted by Anders Broman.

## Checker rules must model the semantics of the API variant

A static checker must understand what each API argument means rather than inferring correctness from superficial type or width relationships. In particular, APIs that decode bytes into a registered field have different source-span requirements from APIs that add an already decoded value while merely associating it with source bytes for highlighting/provenance.

Merged master MR !15026, authored and merged by Martin Mathieson, corrects `check_typed_item_calls.py` so it does not apply a fixed-width source-length warning to `proto_tree_add_uint()`. That API receives the numeric value separately, so the tvbuff span need not have the same width as the destination field. Closed MR !15027 is useful negative evidence: it attempted to change several source spans to four bytes solely to satisfy the warning because the destination fields were `FT_IPv4`; Martin rejected that reasoning because the original source can legitimately be a full IPv6 address whose decoded IPv4 value is supplied separately. The checker, not the dissector, was wrong in that case.

**Implementation rule:** encode checker rules per API family and parameter contract. Do not assume that a registered field's storage/display width dictates the length of every source span associated with that field when the value is passed explicitly.

**Review rule:** when a checker warning suggests changing packet offsets or lengths, confirm that the warning models the called API's semantics before modifying wire parsing. A false-positive checker rule can otherwise turn valid source provenance into an actual dissection bug.

**Confidence:** Very high. Accepted checker correction authored and merged by Martin Mathieson, reinforced by a directly related closed patch whose proposed wire-length changes were rejected as semantically incorrect.