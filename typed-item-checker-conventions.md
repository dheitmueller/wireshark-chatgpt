# Typed Item Checker Conventions

This file records durable conventions for Wireshark's source-level typed-item checks and their relationship to proto-tree APIs. Current checker behavior remains authoritative.

## Run bitmask-field validation continuously, not only during ad hoc review

Bitmask metadata is part of a dissector's decoding contract. Mechanical checks for invalid or suspicious masks are most useful when they run in normal CI for proposed commits rather than depending on a reviewer to remember a special checker mode.

Merged MR !11689, authored and merged by Martin Mathieson, adds `--check-bitmask-fields` to the GitLab CI invocation of `tools/check_typed_item_calls.py`, alongside the existing consecutive-filter, label, mask, and commit-scoped checks. This turns bitmask validation into routine repository validation.

**Testing rule:** before submitting dissector changes, run the same typed-item checks expected by CI, including `--check-bitmask-fields`. Treat failures as field-definition/API-contract issues to investigate rather than cosmetic lints.

**Confidence:** Very high. Merged CI policy change authored and merged by Martin Mathieson.

## Static checker rules must model valid proto-tree idioms precisely

A checker should reject semantically inconsistent field metadata without flagging a legitimate API pattern merely because it resembles the invalid case. For `proto_tree_add_bitmask()`-style field lists, an all-bits-set mask can be valid when that field is the first and only header field representing the entire value; it should not be rejected by a rule intended to catch overlapping or nonsensical component masks.

Merged MR !11698, authored and merged by Martin Mathieson, adds exactly this exception to `check_typed_item_calls.py`: an all-set mask is accepted when it is the first and sole `hf_` item in the bitmask list. The change refines the checker instead of weakening bitmask checking globally.

**Checker-design rule:** encode exceptions in terms of the API shape that makes them valid. Prefer a narrow structural exception over disabling or broadly suppressing a correctness check.

**Review rule:** when a new checker produces findings in existing dissectors, separate true metadata bugs from established valid proto-tree idioms. Fix the dissector for the former and improve the checker model for the latter.

**Confidence:** Very high. Merged checker refinement authored and merged by Martin Mathieson immediately around the CI rollout of bitmask validation.

## Audit every use of a shared field before changing its metadata

A typed-item checker warning describes a mismatch between a particular call site and the registered field contract, but an `hf_` entry can be shared by multiple call sites. Changing the field type, width, display value, or mask solely to satisfy one warning can make the other uses incorrect.

Merged master MR !10605 fixes several BGP findings from the typed-item checker. For the MPLS label-stack fields, the accepted change aligns both the access width and the registered metadata with the full three-byte masked value. During review, Martin Mathieson explicitly rejected a proposed change for another field because the same symbol was used by two other, wider fields. His review demonstrates that checker remediation requires auditing the complete use set rather than mechanically editing the registration nearest the reported warning.

**Review rule:** when a checker reports an `hf_` mismatch, search all uses of that field before deciding whether the defect is in the registration or in one call site. Choose metadata that represents the field's real value domain and make each access compatible with that contract; split the field when apparently shared uses actually have different semantics.

**Testing rule:** rerun the checker after the correction and inspect all call sites of any registration whose type, mask, or width changed. A warning disappearing at one location is not sufficient evidence that the shared field is now correct.

**Confidence:** High. Merged master checker-driven fix with direct review from Martin Mathieson about the risks of changing a field shared by wider accesses.


## Passing a checker does not prove protocol semantics

Merged MR 10493 made broad typed-item checker driven corrections, but Lars Volker later reported that the change had broken at least one dissector. This reinforces the existing all-use audit rule: a checker can prove that metadata and call patterns satisfy its model, but it cannot prove that the resulting field width, mask, or interpretation matches the protocol.

**Review rule:** after checker-driven metadata changes, verify the protocol semantics and all affected call sites, and run representative behavior tests where practical.

**Confidence:** High. This is a concrete regression report attached to a merged checker-cleanup MR and directly corroborates the stronger MR 10605 rule above.

## The typed-item checker has long been intended as pre-submit feedback, not a substitute for semantic review

Merged MR !6871 expanded the CI invocation of `check_typed_item_calls.py` with the then-current `--consecutive --label --mask` checks while deliberately leaving findings warning-only. The same MR's review also clarifies that fixing an MR title does not change the commit message inspected by commit checks; the contributor had to amend and push the commit itself.

**Workflow rule:** run the typed-item checker locally over the proposed commits, but treat its output as structural/API feedback that still requires protocol-semantic review. When CI complains about commit metadata, modify the actual commit object rather than only the GitLab MR title.

**Historical note:** !6871 records the checker flags used in 2022. Use the repository's current checker command for present-day submissions; later notebook entries record additional options such as `--check-bitmask-fields`.

**Confidence:** High. Merged CI/tooling change with direct review from Alexis La Goutte.



## Review warning-only checker output even when CI is green

Merged master MR !4094 fixes a duplicate display-filter abbreviation. Martin Mathieson demonstrates `check_typed_item_calls.py --consecutive --file ...`, which also reports several field-width warnings in the same dissector, and explains that CI intentionally fails only obvious or clear checker errors so contribution friction stays reasonable.

**Workflow rule:** run the checker locally on touched dissectors and inspect warning-only findings even when the pipeline passes. A green CI result means the hard-error threshold was not crossed; it does not mean there are no structural inconsistencies worth reviewing.

**Historical note:** !4094 records an early single-file invocation. For current submissions use the repository's current command/options, including the later-established commit-range and bitmask checks recorded elsewhere in this notebook.

**Confidence:** Very high. Merged correction with direct maintainer explanation from Martin Mathieson.
