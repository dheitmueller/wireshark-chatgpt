# Wireshark Contribution Licensing and Provenance Conventions

This file records practical contribution-review conventions around externally sourced code and data. It is not legal advice; current Wireshark project policy, accepted SPDX/license checks, and maintainer guidance remain authoritative.

## Do not import useful external code or data without acceptable contribution provenance

A technically useful table, mapping, generated artifact, or source fragment is not automatically suitable for inclusion in Wireshark merely because its contents are public or available in another open-source tree. The license and provenance of the actual material being incorporated are part of the submission contract.

Merged master MR !21396 adds human-readable Darwin kernel drop-reason mappings. The desired mappings existed in Apple's XNU source under APSL terms, so the contributor did not simply copy the original file into Wireshark. Apple instead provided a separate trimmed mapping specifically for contribution. During review, the initially proposed `GPL-2.0-only` license was rejected as unsuitable for Wireshark's project requirements and was also rejected by `tools/checklicenses`; after further review the contributed mapping was licensed under MIT and the MR merged. Release backport !21440 carries the accepted result.

**Submission rule:** when a change depends on externally sourced code, tables, constants, or other copyrightable material, verify that the material being submitted has provenance and license terms acceptable to Wireshark before treating the implementation as merge-ready. If the original source's terms are unsuitable, obtain an independently contributed or appropriately relicensed representation rather than copying the original and trying to work around the checker. Keep the resulting license/provenance explicit, run the project's license checks, and escalate genuine licensing questions to maintainers/project counsel rather than inferring compatibility from technical similarity.

**Confidence:** Very high as a Wireshark contribution-process rule. Merged master MR with explicit Gerald Combs review, project license-checker enforcement, contributor/legal provenance work, and a stable-branch backport. The specific license-compatibility outcome is intentionally not generalized beyond this evidence.

## Imported algorithm implementations need file-level provenance and standalone validation

When Wireshark imports an external algorithm implementation rather than merely calling an external library, review has two independent gates: the imported source must have acceptable, traceable licensing/provenance, and the algorithm must be testable independently of the dissector that will consume it.

MR !8973, opened in 2022 and ultimately merged on August 12, 2026, adds SAP LZC/LZH decompression support to `wsutil` using code derived from ClamSAP/SecureAuth sources. Alexis La Goutte blocked progress on license-check failures, including an incorrect historical FSF address and an unrecognized header, until the source/header provenance was normalized. Later review deliberately kept the decompressor's unit tests rather than bypassing a failing test: Michael Mann noted that removing the test would merely hide a likely protocol failure, and the contributor corrected the ported test data. John Thacker also requested typed `wmem_new0(..., CSHDL)` allocation for the single state object. Only after those issues were resolved did the reusable library merge, allowing a separate SAP dissector MR to depend on it.

**Submission rule:** for imported algorithm code, keep per-file license/provenance information acceptable to Wireshark's license checker and preserve the imported implementation behind a narrow project-native wrapper where practical. Add direct unit tests for the algorithm before wiring it into protocol dissectors; do not treat a downstream dissector as the only validation surface.

**Confidence:** Very high. Ultimately merged reusable library with explicit Alexis La Goutte licensing review, Michael Mann test scrutiny, and John Thacker implementation review.
