# Wireshark Test-Runner Integration Conventions

This file records durable conventions for invoking Wireshark's test suite from CI and other automation. Current upstream build and test configuration remain authoritative.

## Invoke tests through the project's supported integration layer

A test suite may contain Python tests without being a pytest-native suite. CI should invoke the test layer that defines Wireshark's environment, selection, setup, and result semantics rather than bypassing it based only on the implementation language of the tests.

Merged release-4.0 MR !16418, authored by Gerald Combs and approved/merged by John Thacker, replaces a direct `pytest-3` CI invocation with CTest. The MR states explicitly that Wireshark's test framework is not compatible with pytest; CTest runs the supported `python3 test.py` entry point and preserves the project's intended harness behavior.

**Implementation rule:** use CTest/the configured Wireshark test entry points in CI unless a particular test target is explicitly documented as directly runnable by another framework. Do not infer that a Python test suite should be launched with pytest merely because pytest can discover some of its files.

**Confidence:** Very high. Accepted stable-branch CI correction authored by Gerald Combs and approved/merged by John Thacker.