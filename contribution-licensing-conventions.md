# Wireshark Contribution Licensing and Provenance Conventions

This file records practical contribution-review conventions around externally sourced code and data. It is not legal advice; current Wireshark project policy, accepted SPDX/license checks, and maintainer guidance remain authoritative.

## Do not import useful external code or data without acceptable contribution provenance

A technically useful table, mapping, generated artifact, or source fragment is not automatically suitable for inclusion in Wireshark merely because its contents are public or available in another open-source tree. The license and provenance of the actual material being incorporated are part of the submission contract.

Merged master MR !21396 adds human-readable Darwin kernel drop-reason mappings. The desired mappings existed in Apple's XNU source under APSL terms, so the contributor did not simply copy the original file into Wireshark. Apple instead provided a separate trimmed mapping specifically for contribution. During review, the initially proposed `GPL-2.0-only` license was rejected as unsuitable for Wireshark's project requirements and was also rejected by `tools/checklicenses`; after further review the contributed mapping was licensed under MIT and the MR merged. Release backport !21440 carries the accepted result.

**Submission rule:** when a change depends on externally sourced code, tables, constants, or other copyrightable material, verify that the material being submitted has provenance and license terms acceptable to Wireshark before treating the implementation as merge-ready. If the original source's terms are unsuitable, obtain an independently contributed or appropriately relicensed representation rather than copying the original and trying to work around the checker. Keep the resulting license/provenance explicit, run the project's license checks, and escalate genuine licensing questions to maintainers/project counsel rather than inferring compatibility from technical similarity.

**Confidence:** Very high as a Wireshark contribution-process rule. Merged master MR with explicit Gerald Combs review, project license-checker enforcement, contributor/legal provenance work, and a stable-branch backport. The specific license-compatibility outcome is intentionally not generalized beyond this evidence.

## Reused third-party decompression code needs provenance checks and real output tests

Merged master **!8973** adds SAP LZC/LZH decompression using a ClamSAP-derived implementation, consolidated into `wsutil` rather than a one-off protocol copy. **Alexis La Goutte** identified a failed license scan: incorrect FSF address in GPL headers and an unknown-license imported header. The contributor replaced the ambiguous header with a properly documented counterpart and corrected licensing details before merge. **Michael Mann** insisted on retaining the failing LZC unit test: a known test-vector formatting error and actual decode mismatch had to be resolved rather than deleting the test. **John Thacker** requested `wmem_new0(scope, CSHDL)` for a typed single object instead of allocating an untyped byte block.

**Rule:** import only code whose license and provenance pass Wireshark's exact source checks. A license tag alone does not cure ambiguous third-party licensing. Put reusable algorithms at the appropriate utility layer and test actual decompressed bytes for both supported variants, expected errors and small/invalid output buffers. Respect native allocator types, not just byte counts.

**Confidence:** Very high for merged library plus Alexis, Michael Mann and John Thacker's explicit technical and licensing feedback.
