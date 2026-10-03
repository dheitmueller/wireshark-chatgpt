# Wireshark Dependency Version Compatibility Conventions

This file records durable conventions for compatibility workarounds whose necessity depends on behavior introduced by an external dependency. Current upstream source and dependency release histories remain authoritative.

## Version guards must follow the first affected dependency release, including backports

Do not assume that a behavioral change begins only with the dependency's next major or minor release. Upstream projects can backport changes, including regressions, to older maintenance lines. A Wireshark workaround should therefore be guarded by the first dependency release that actually contains the relevant behavior.

Merged master MR !14992, authored and merged by John Thacker, fixes the Qt SyntaxComboBox workaround after the Qt change responsible for the background-color regression was backported to Qt 5.15.3. Wireshark's previous version condition covered later Qt versions but not that backported maintenance release, leaving Qt 5.15.3 affected. The accepted change extends the workaround to the real first affected release. Stable-branch MRs !14993 and !14994 carry the same correction downstream.

**Implementation rule:** derive compatibility predicates from the dependency's actual change history, not from an assumed major/minor boundary. When a workaround corresponds to a known upstream issue or commit, document that provenance and include backported releases in the affected range.

**Review/testing rule:** for a version-specific compatibility change, identify the oldest affected supported dependency release and the first unaffected release where practical. Test or otherwise verify the boundary versions, especially when distributions may ship maintenance releases containing backported behavior.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker, with explicit upstream Qt issue/backport rationale and stable-branch propagation.

## Source and test syntax must remain valid on every supported runtime version

Moving one platform to a newer interpreter or dependency does not automatically raise Wireshark's minimum version everywhere. Shared scripts and tests must therefore stay within the syntax and standard-library feature set of the oldest runtime that the project still supports, unless the project intentionally changes that minimum.

Merged master MR !14616 moves Windows builds to Lua 5.3. During review, John Thacker caught test code that used Lua floor-division syntax introduced in 5.3. Because the same test suite still had to run with older supported Lua versions, he suggested the version-neutral equivalent `math.floor(milli/1000)`. The concern was resolved before merge.

**Implementation rule:** when upgrading a dependency on one build target, distinguish that target's selected version from the repository-wide minimum supported version. New shared code must not rely on syntax or APIs introduced after the minimum unless the compatibility policy is being changed deliberately.

**Review/testing rule:** dependency-upgrade MRs should run or reason about shared tests under both the newly selected version and the oldest still-supported version. Pay special attention to parser-level syntax changes: a runtime cannot execute a compatibility branch if it cannot parse the file in the first place.

**Confidence:** High. Merged dependency transition authored by Anders Broman with a concrete cross-version syntax problem caught by John Thacker during review and corrected before merge.

## Raise minimum dependency versions from the supported-platform floor

The useful feature level of a dependency and the minimum version Wireshark can require are different questions. A minimum-version bump should be justified by the versions guaranteed across the project's supported platform/distribution matrix; requiring a newer release merely because it is more capable can drop an otherwise supported environment.

Merged master MR !1543, authored by John Thacker, raises the libgcrypt minimum from 1.4.2 to 1.5.0 after RHEL/CentOS 6 became unsupported. The MR explicitly stops short of 1.6.0 despite its significant improvements because RHEL/CentOS 7 supplied libgcrypt 1.5.3. Once 1.5.0 became the supported floor, the accepted change removed Wireshark's fallback AES key-unwrapping implementation that was only needed for older libgcrypt.

**Compatibility rule:** choose a dependency minimum no higher than the version guaranteed by every platform the project still supports, unless dropping a platform is itself an intentional project decision. Re-evaluate that floor when the support matrix changes.

**Cleanup rule:** when a higher minimum makes compatibility code unreachable, remove the obsolete fallback in the same transition when practical so the new dependency contract is explicit in both build configuration and source.

**Confidence:** Extremely high. Merged master dependency-policy cleanup authored by John Thacker with the supported-distribution boundary and deliberate refusal to require 1.6 stated directly in the MR.


## Probe dependency header layout instead of assuming one version's file organization

Merged master MR !228 fixes libssh version detection after libssh 0.9.5 moved version macros from `libssh.h` to `libssh_version.h`. Wireshark checks for the newer header and falls back to the older header when it is absent; merged !229-!231 carry the same fix to release branches.

**Detection rule:** when a supported dependency moves equivalent API or version metadata between headers, probe the installed layout and select the appropriate source rather than hard-coding the newest location.

**Compatibility rule:** preserve a known working legacy fallback while the project still supports releases using it. The build check should answer the concrete capability or layout question that determines how to proceed.

**Confidence:** Very high. Merged master build fix with three accepted release-branch backports.


## Align optional-dependency declarations with the version/capability guards around their users

A dependency guard that removes every use of a static object can still leave a build failure if the declaration remains unconditional and the project's strict warning configuration treats the resulting unused object as an error.

Merged master MR !5 fixes this for old-libgcrypt builds by moving Bluetooth Mesh reassembly descriptors inside the matching libgcrypt version guard and QUIC stream-fragment descriptors inside `HAVE_LIBGCRYPT_AEAD`. The functional code was already guarded; the missing part was the declaration boundary.

**Compatibility rule:** when supporting multiple dependency versions or optional capabilities, audit the whole translation unit under both sides of each guard. Static constants, helper tables, and functions whose only consumers are feature-gated should normally be gated with those consumers.

**Testing rule:** build representative supported dependency configurations with strict warnings enabled. Feature-enabled success does not cover the feature-disabled source shape.

**Confidence:** High. Merged master build-portability fix motivated by a real supported old-libgcrypt configuration.
