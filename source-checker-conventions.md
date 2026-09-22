# Wireshark Source-Checker Conventions

This file records durable conventions for Wireshark's repository-specific source checking scripts. Current upstream tools and compiler diagnostics remain authoritative.

## Model the semantic context of a checked field instead of applying one blanket rule

A project-specific checker should validate the contract that applies at the actual call/data-structure site. A field can have different valid metadata requirements depending on how it is consumed, so a checker that ignores that context can both miss real bugs and recommend an incorrect "fix."

Merged master MR !13599, authored and merged by Martin Mathieson, strengthens `check_typed_item_calls.py` specifically for fields passed in bitmask field arrays. In that context, a member item is expected to carry a non-zero mask; the change both diagnoses zero-mask members and stops the checker from suggesting that those fields should have a zero mask. The MR also documents the practical limitation that source-level checking cannot resolve every non-literal or complex macro expression.

**Implementation rule:** encode the semantic role of the item being checked—such as membership in a bitmask field array—before deciding which mask/type/display rules apply. Prefer a narrower check with known parse limitations over a broad rule that produces semantically wrong advice.

**Review rule:** when extending a source checker, run it over the existing tree and inspect both new positives and newly suppressed cases. A warning is useful only if its suggested invariant is valid for the API pattern that triggered it.

## Narrow false-positive exceptions without weakening genuine width/type checks

Repository checkers should distinguish a protocol-valid exceptional pattern from the actual unsafe condition they are trying to catch. If a warning is proven to be a false positive, adjust the checker or representation narrowly rather than disabling the whole class of check.

Merged master MR !13574 exposed two useful examples in review. The RDP `SEC_PKT_MASK` value `0x4c1` is intentionally non-contiguous, so a generic "non-contiguous mask" warning was not appropriate for that established field. A separate warning was triggered by the spelling `0x08000` for an `FT_UINT16` mask: the value fits in 16 bits because the fifth hexadecimal digit is zero, although a non-zero fifth nibble would genuinely exceed the field width. Martin Mathieson created merged MR !13576 to avoid the non-contiguous-mask false positive, while the field spelling was simplified to `0x8000` so the real width check remained meaningful.

**Implementation rule:** suppress or special-case only the condition demonstrated to be valid. Preserve checks that would still detect a materially different unsafe value, such as a mask whose significant bits exceed the registered field width.

**Review rule:** do not equate textual shape with numeric semantics when a checker is reasoning about integer width or masks. Leading zeroes, resolvable macros, and protocol-defined non-contiguous masks can require more precise analysis; conversely, a known exception should not become an excuse to disable neighboring checks.

**Confidence:** High. Both rules come from merged master checker work by Martin Mathieson and from direct buildbot/review discussion that distinguishes specific false positives from real mask-width errors.

## Keep whole-tree and changed-file checker modes aligned on the same source domain

A repository checker should not silently examine a narrower source set in its normal whole-tree mode than it examines in a commit/range mode. Otherwise the same source file can be clean in an ordinary local run but start producing findings only after it appears in the changed-file set, making coverage depend on invocation mode rather than repository semantics.

Merged master MR !13547, authored by Martin Kaiser and merged by Martin Mathieson, fixes exactly this in `check_tfs.py`. `--commits 500` could process `epan/dissectors/asn1/gsm_map/packet-gsm_map-template.c` because it was changed in the selected history, while an unqualified whole-tree run only scanned files directly under `epan/dissectors/` and skipped dissector subdirectories. The accepted fix recursively traverses those subdirectories so the two modes cover the same class of dissector sources.

**Implementation rule:** define a checker's semantic source domain once and make every invocation mode select from that domain. Incremental/commit filtering may reduce the set to changed files, but an unfiltered whole-tree run should be at least as inclusive and must include nested dissector, template, or generated-source locations that the incremental path can legitimately reach.

**Review rule:** when adding a new checker mode or directory layout, compare the file lists produced by whole-tree and changed-file invocations. A checker that reaches a file only through one mode has a coverage bug even if each mode appears internally consistent.

**Confidence:** Very high. The inconsistency and concrete missed nested dissector file are stated directly in merged master MR !13547, and the accepted change was specifically to make the outputs consistent.