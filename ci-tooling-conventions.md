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
