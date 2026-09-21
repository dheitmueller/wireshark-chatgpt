# Wireshark Dependency Version Compatibility Conventions

This file records durable conventions for compatibility workarounds whose necessity depends on behavior introduced by an external dependency. Current upstream source and dependency release histories remain authoritative.

## Version guards must follow the first affected dependency release, including backports

Do not assume that a behavioral change begins only with the dependency's next major or minor release. Upstream projects can backport changes, including regressions, to older maintenance lines. A Wireshark workaround should therefore be guarded by the first dependency release that actually contains the relevant behavior.

Merged master MR !14992, authored and merged by John Thacker, fixes the Qt SyntaxComboBox workaround after the Qt change responsible for the background-color regression was backported to Qt 5.15.3. Wireshark's previous version condition covered later Qt versions but not that backported maintenance release, leaving Qt 5.15.3 affected. The accepted change extends the workaround to the real first affected release. Stable-branch MRs !14993 and !14994 carry the same correction downstream.

**Implementation rule:** derive compatibility predicates from the dependency's actual change history, not from an assumed major/minor boundary. When a workaround corresponds to a known upstream issue or commit, document that provenance and include backported releases in the affected range.

**Review/testing rule:** for a version-specific compatibility change, identify the oldest affected supported dependency release and the first unaffected release where practical. Test or otherwise verify the boundary versions, especially when distributions may ship maintenance releases containing backported behavior.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker, with explicit upstream Qt issue/backport rationale and stable-branch propagation.
