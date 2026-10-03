# Platform and Bootstrap Conventions from !1010-!1059

## Verify feature enablement in the resulting binary and account for stale dependency builds

Merged master MR !1039 adds PKCS #11 support to the macOS support-library build. John Thacker supplies feature-specific decryption tests and points to `tshark -v` as an observable capability check. Jörg Mayer confirms that a previously built GnuTLS must be rebuilt before the new configuration appears.

**Rule:** when dependency configuration changes to enable a feature, define the transition for existing cached support libraries and verify that the final Wireshark binary exposes the capability. A successful dependency build is not enough.

## Parse platform versions by semantic components

Merged master MR !1038, authored and merged by Guy Harris, fixes `macos-setup.sh` after macOS moved beyond the `10.N` numbering era. The accepted code extracts major and minor fields and compares those values.

**Rule:** compare the semantic version components the policy needs; do not treat a vendor's historical numbering prefix as a permanent grammar.

## Shared support images follow all maintained branches

Merged !1055 removes Wireshark's final Bison/YACC grammar from master and deletes the corresponding master build, packaging, and documentation dependency. Guy Harris notes that shared support images still need Bison because maintained 3.4 and 3.2 branches continue to consume it.

**Rule:** removing the final consumer on master does not automatically remove a dependency from shared CI or bootstrap infrastructure. Evaluate those images against every maintained branch they serve.
