# Wireshark Test Environment Conventions

This file records durable conventions for keeping Wireshark automated tests deterministic and independent of the developer's personal configuration. Current upstream test infrastructure remains authoritative.

## Run command-line integration tests with the controlled test environment

Tests that invoke Wireshark command-line applications such as `tshark` must not accidentally inherit the user's active profile, disabled-protocol list, preferences, or other personal configuration. Use the test suite's defined environment for subprocesses so the expected behavior comes from repository test inputs and explicit test setup rather than the machine on which the test happens to run.

Merged master MR !23993 fixes tests whose results could change when protocols were disabled in the current user's configuration by passing the existing `test_env` to the affected subprocess invocations. Merged release-4.6 backport !24006 carries the same correction to the stable branch, strengthening the evidence that configuration isolation is part of the test contract rather than a one-off local workaround.

**Implementation rule:** when a test launches Wireshark executables, use the suite-provided controlled environment unless the purpose of the test is specifically to exercise user-environment behavior. A green test should not depend on the developer's profile or local preferences.

**Confidence:** Very high. Merged master fix with an accepted stable backport.