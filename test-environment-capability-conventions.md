# Test environment capability conventions

## Enumerate expected capability-dependent failures

Tests that probe capture or interface capabilities must distinguish an unavailable optional backend from a product regression. If the same command has several legitimate outcomes depending on installed capture support, enumerate those specific semantic exit codes instead of assuming one universal result or accepting arbitrary failure.

Merged master MR !14851, authored and merged by John Thacker, updated `test_tshark_interface_chars` so systems without Npcap may return `INVALID_INTERFACE` in addition to `OK`, `PCAP_ERROR`, and `INVALID_CAPABILITY`. This made the test reflect valid MSYS2 and other Windows build environments without weakening it to accept an arbitrary nonzero status.

Keep environment-dependent expectations narrow and named. Model the actual capability boundary and its documented failure modes; do not hide unrelated regressions behind blanket success-or-failure acceptance.

**Confidence:** High. Merged master test correction authored and merged by John Thacker.
