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

## Checker parsing must allow valid source formatting and still verify encoding semantics

Martin Mathieson's merged !9592 fixes TWAMP MBZ fields whose registered `FT_UINT8` type disagreed with a two-byte item span. His review diagnosed why `tools/check_typed_item_calls.py` missed the calls: the source contains whitespace between the function name and opening `(`. Merged !9593 changes the checker's regular expression to allow that whitespace; merged !9562 broadens encoded-value checking and repairs `proto_tree_add_item` calls across multiple dissectors.

**Rule:** source checkers must recognize syntactically valid formatting, not just a narrow canonical spelling. Test the checker with whitespace variants and ensure the `ENC_*` mode, registered `FT_*` field type, mask, and tvbuff item width agree with the actual protocol. Use checker reports to guide semantic inspection rather than treating regex success as proof. **Confidence:** high; Martin Mathieson's merged diagnostic/fix chain.

## Roll out disputed checker style rules as opt-in diagnostics

Merged master !9086, authored by Martin Mathieson, adds `--mask-exact-width` to `tools/check_typed_item_calls.py` to warn when the textual hexadecimal mask width of an `FT_BOOLEAN` does not match its declared number of bits. This makes full-width bit positions easier to read, but it originally identified **564 warnings** in roughly 1,600 dissectors. Jaap Keuter questioned treating leading-zero formatting as mandatory; Martin deliberately kept the check **off by default**, rather than imposing noisy pipeline failures. It simultaneously fixed actual mask/field-width errors (including WLAN) and improved the checker's source parsing.

**Review rule:** distinguish semantic impossibilities (bit outside field width, wrong registered field width, mask overlap) from readability preferences such as zero padding. Introduce optional warnings and measure the legacy-warning baseline before turning style policy into mandatory CI. Check representative dissectors manually, not just regex results. Related merged !9072 corrects USB-CCID's `wLevelParameter` from `FT_UINT8` to `FT_UINT16`, demonstrating a real semantic error amid formatting changes. **Confidence:** very high for accepted checker behavior; a preference, not a universal upstream requirement, for padding all masks.
