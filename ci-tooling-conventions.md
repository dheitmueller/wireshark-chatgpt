# Wireshark CI and Tooling Conventions

This file records durable continuous-integration and tool-integration conventions extracted from accepted upstream Wireshark changes. Current upstream CI configuration and tools remain authoritative.

## Gate a downstream report step on the exact artifact it consumes

A conditional producer does not guarantee that every downstream representation exists. When one CI command consumes a generated file, test that generated file itself before invoking the consumer rather than reusing a condition on an earlier source/intermediate artifact.

Merged MR !26194, authored, approved, and merged by John Thacker, fixes the cppcheck HTML-report stage. The XML report was generated only conditionally from an earlier text report, but `cppcheck-htmlreport` was guarded by the text file rather than by its actual XML input. The accepted pipeline checks `cppcheck_report.xml` directly before running the HTML converter; this also unblocked the documentation MR !26190 whose pipeline had exposed the mismatch.

**Implementation rule:** each optional pipeline stage should predicate execution on the exact input artifact or state it consumes. Avoid proxy conditions that merely happened to correlate with that artifact in an earlier version of the pipeline.

**Confidence:** Very high. Merged master CI fix authored and merged by John Thacker with a concrete pipeline failure.

## Integrate tools according to their real stdout/stderr contract

Do not assume that a command-line analysis tool writes its primary report to stdout and diagnostics to stderr. Verify the tool's actual stream behavior, then route streams deliberately so machine/report output can be transformed or captured without hiding syntax, option, or setup errors from the job log.

Merged MR !26197, authored, approved, and merged by John Thacker, documents that cppcheck writes its analysis report to stderr while some option/parsing errors appear on stdout. Wireshark wants to colorize/tee the report while still displaying those errors, so the accepted shell wrapper explicitly swaps stdout and stderr around the cppcheck invocation before piping the report through `colorize`.

**Implementation rule:** treat stdout/stderr routing as part of a tool adapter's API contract. Preserve both the report stream and diagnostic stream intentionally; do not collapse them with `2>&1` when downstream processing needs to distinguish them.

**Confidence:** Very high. Merged master tooling fix authored and merged by John Thacker, with the counterintuitive upstream stream behavior explained in the commit.

## Encode cheap structural conventions in repository checkers

When a recurring code-quality issue can be recognized mechanically with low ambiguity, prefer extending Wireshark's existing source checker so new instances are caught during normal development and CI rather than relying on reviewers to remember the convention manually.

Merged MR !25259, authored by Martin Mathieson and merged by Anders Broman, extends `check_typed_item_calls.py` to detect duplicated hand-written `value_string` definitions. The discussion notes an important scope distinction: there were only hundreds of such duplicates in hand-written files but nearly two million occurrences in generated files, where deduplication belongs in the generator and may require a naming strategy rather than applying the hand-written-source check blindly.

**Implementation rule:** automate deterministic, inexpensive review checks in the closest existing project checker. Keep generated code on an explicit path: either teach the generator to satisfy the invariant or exempt generated output when applying the source-level rule directly would create huge low-value noise.

**Confidence:** High. Merged master tooling change by a long-time Wireshark maintainer, with the generated-versus-hand-written distinction discussed explicitly before merge.

## CI must install the dependencies needed for tests it intends to count

A green job is not meaningful coverage if the relevant tests silently self-skip because an optional test-only module is absent. When a CI configuration is intended to exercise a test family, its environment must provide that family's dependencies or otherwise make the missing coverage visible.

Merged MR !25222 adds `jsonschema` to Linux and Windows CI environments because the theme schema tests self-skipped without it; those jobs had therefore appeared successful while not executing the intended schema validation. The fix was merged by Anders Broman.

**Implementation rule:** audit conditional skips in test suites against each CI image. If a job is supposed to validate a feature, install the required test dependency and prefer CI output that distinguishes intentional platform exclusions from accidental missing-dependency skips.

**Confidence:** Very high. Merged master CI correction with the silent-skip failure mode stated explicitly and Anders Broman approval.