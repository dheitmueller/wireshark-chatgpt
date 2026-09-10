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

## Sanitizer builds must not enable hardening that masks sanitizer findings

Security hardening and sanitizer instrumentation are both useful, but they are not automatically compatible. A sanitizer CI configuration should be optimized for exposing the class of bugs that sanitizer is intended to find; compiler or libc hardening that intercepts the same failure first can make the sanitizer job less useful.

Merged MR !25005, authored and merged by John Thacker and approved by Gerald Combs, stops defining `_FORTIFY_SOURCE` in AddressSanitizer and ThreadSanitizer builds. The MR explains that fortified libc checks can terminate first and prevent ASan from reporting the underlying memory error with its more useful diagnostics.

**Implementation rule:** evaluate hardening flags separately for sanitizer configurations. If a hardening mechanism masks or preempts the sanitizer's diagnostic path, disable it for that sanitizer build rather than assuming that more instrumentation is always better.

**Confidence:** Very high. Merged master CI/build correction authored and merged by John Thacker with Gerald Combs approval.

## Fuzz-only guardrails should fail in a form the fuzz harness can classify

A defensive parser guard that merely throws a recoverable dissector exception may protect an interactive dissection without producing a useful fuzzing finding. When a guard exists specifically to detect pathological non-progress or resource behavior under the fuzz configuration, it should terminate in the failure mode expected by the fuzz harness so the input is retained and triaged.

Merged MR !24991, authored and merged by John Thacker, changes the too-many-idle-items guard to call `ws_error()` when `WIRESHARK_ABORT_ON_TOO_MANY_ITEMS` is enabled. In normal operation the softer dissector exception remains appropriate; under fuzzing, the hard abort turns the suspected infinite-loop/resource pathology into an actionable fuzz failure.

**Implementation rule:** distinguish user-facing recovery behavior from fuzz-oracle behavior. Keep ordinary malformed-packet handling recoverable where appropriate, but make fuzz-specific non-progress/resource guards fail strongly enough for the harness to detect, preserve, and report the triggering input.

**Confidence:** Very high. Merged master fuzzing behavior authored and merged by John Thacker.

## Do not suppress the diagnostic severity that makes a CI tool fail

A CI job that is configured to fail on warnings must still print those warnings. Logging options are part of the failure contract: suppressing the same severity that drives the exit status produces opaque failures and wastes review/debugging time.

Merged MR !24983, authored by John Thacker, removes Asciidoctor's `--quiet` option because Wireshark also uses a warning failure level. A documentation job had failed for days on an unsupported GIF while the warning explaining the failure was hidden from the job log.

**Implementation rule:** whenever a tool uses a warning/error threshold to decide job success, ensure the configured verbosity leaves that threshold visible. Prefer concise logs, but never silence the diagnostic class that can make the job red.

**Confidence:** Very high. Merged master CI diagnosis fix authored by John Thacker with a concrete multi-day failure example.

## Pin unreleased VCS tooling to an immutable revision

When CI needs a capability that has landed in a tool's upstream repository but is not yet available in its published package, installing directly from version control can be appropriate, but the repository reference must itself be reproducible. Tracking a moving branch would make identical Wireshark commits consume different tooling over time.

Merged MR !25830, authored by Gerald Combs and approved and merged by John Thacker, updates the Windows Qt-installation path because the PyPI release of `aqtinstall` did not yet support Qt 6.11. The accepted configuration points `WS_AQTINSTALL_LOCATION` at the upstream Git repository pinned to the exact commit `5be32a75a81513792d95f13065b72cac3381c612` and installs that revision in the CI virtual environment.

**Implementation rule:** if CI temporarily consumes unreleased tooling directly from version control, pin it to an immutable commit or equivalently immutable artifact and centralize the reference so it can be audited and replaced when an appropriate packaged release appears. Do not replace a missing release with nondeterministic branch-tip installation.

**Confidence:** Very high. Merged build-infrastructure change authored by Gerald Combs and approved/merged by John Thacker.
