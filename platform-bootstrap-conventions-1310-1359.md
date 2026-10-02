# Platform Bootstrap Conventions from MRs 1310-1359

These rules supplement the broader platform and build convention files. Current upstream source remains authoritative.

## Preserve the selected build environment

Merged master MR !1311, authored by Guy Harris, ensures that a platform discovery helper uses the SDK already selected by Wireshark's setup script rather than silently falling back to the host default. Release backports !1319 and !1321 carry the same behavior. Guy's merged !1329 similarly makes a support-file lookup independent of the current build directory by anchoring it to the canonical source root.

**Rule:** once bootstrap code selects an SDK, toolchain, source root, or build root, propagate that choice through subordinate discovery and file-resolution operations. Do not let implicit host defaults or the current directory select a different environment.

## Follow each dependency's real lifecycle

Guy Harris's merged master work in !1350, !1353, !1356, !1316, and !1318, together with release-branch backports, shows that dependencies do not all expose the same setup and cleanup operations. Wireshark's bootstrap code adapts to the lifecycle each dependency actually provides, keeps installation-state markers consistent with those operations, and deliberately regenerates build machinery when that is required to obtain the install/cleanup behavior Wireshark needs.

**Rule:** treat dependency setup and cleanup as dependency-specific contracts rather than assuming a universal build-system lifecycle. Keep Wireshark's own bookkeeping synchronized with the operations actually available, and document intentional build-system regeneration.

## Report dependency identity only as precisely as it can be observed

Merged master MR !1347, authored by Guy Harris, reports whether Minizip was compiled in but deliberately does not report a version because neither the headers nor runtime API expose a trustworthy version value. Release backports !1348 and !1349 preserve the same behavior.

**Rule:** distinguish presence, capability, compile-time version, and runtime version. Report the strongest fact Wireshark can establish reliably instead of inferring precision from packaging or filenames.

**Confidence:** Extremely high for all three sections. The master precedents are merged Guy Harris-authored changes, with release-branch backports where noted.
