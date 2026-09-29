# Wireshark Test Environment Conventions

This file records durable conventions for keeping Wireshark automated tests deterministic and independent of the developer's personal configuration. Current upstream test infrastructure remains authoritative.

## Run command-line integration tests with the controlled test environment

Tests that invoke Wireshark command-line applications such as `tshark` must not accidentally inherit the user's active profile, disabled-protocol list, preferences, or other personal configuration. Use the test suite's defined environment for subprocesses so the expected behavior comes from repository test inputs and explicit test setup rather than the machine on which the test happens to run.

Merged master MR !23993 fixes tests whose results could change when protocols were disabled in the current user's configuration by passing the existing `test_env` to the affected subprocess invocations. Merged release-4.6 backport !24006 carries the same correction to the stable branch, strengthening the evidence that configuration isolation is part of the test contract rather than a one-off local workaround.

**Implementation rule:** when a test launches Wireshark executables, use the suite-provided controlled environment unless the purpose of the test is specifically to exercise user-environment behavior. A green test should not depend on the developer's profile or local preferences.

**Confidence:** Very high. Merged master fix with an accepted stable backport.


## Normalize configuration-precedence variables in the shared test environment

Merged master MR !5129, authored by Gerald Combs, removes `XDG_CONFIG_HOME` in the common test-environment factory because it takes precedence over `HOME` and could cause Wireshark command-line tests to read the host or CI runner's configuration instead of the temporary test home. Release-3.6 MR !5134 carries the same correction. Merged master !5152 then removes the earlier GitHub Actions-only workaround because the shared fixture now establishes the invariant for every test runner.

**Isolation rule:** normalize environment variables at the shared subprocess/test-fixture boundary when they can override the suite's intended configuration root. Do not fix a cross-runner test invariant only in one CI workflow.

**Precedence rule:** when isolating a test home, audit higher-priority configuration variables as well as `HOME`; setting a lower-priority variable is insufficient if another inherited variable overrides it.

**Maintenance rule:** once the common fixture owns an environment invariant, remove workflow-specific copies so local tests, alternate CI systems, and future runners all exercise the same setup.

**Confidence:** Extremely high. Merged master fix by Gerald Combs, stable propagation, and an explicit merged cleanup reverting the now-redundant workflow workaround.
