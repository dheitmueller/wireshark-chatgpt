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

## Repair field-registration warnings precisely, retaining documented wire-layout exceptions

Merged master !9477 (Martin Mathieson) repairs a large collection of `check_typed_item_calls.py` findings: 16/24/32-bit field masks are written at the corresponding fixed width, repeated GSM SMS abbreviations get distinct protocol-specific names, and a legitimate **noncontiguous** STUN method mask is listed explicitly as a checker exception with RFC 5389 support. Merged !9493 separately fixes an ambiguous duplicate BGP Link State display-filter abbreviation by giving the old-format field its own `.old` suffix; its MR description also documents a mismatched FT_UINT16-versus-four-byte usage that should not be inferred corrected by the one-line diff.

**Checker rule:** validate registration width, item width, abbreviation uniqueness, and mask shape separately. Prefer adjusting actual field registration; use a narrowly named/documented exception for *specification-defined* discontinuous bits rather than disabling all mask checks. Confirm the final diff addresses each warning claimed in the description. **Confidence:** high for merged Martin/Alexis changes; note that accepted checker exceptions do not mean arbitrary noncontiguous masks are correct.

## Check bitfield insertion contracts for the actual registered field types

Merged master !9243 (Martin Mathieson) enables `check_typed_item_calls.py` checks for `proto_tree_add_bits_item()` and `proto_tree_add_bits_ret_val()`, broadening static coverage beyond the conventional typed insertion paths. Merged DECT-NWK fix !9232 supplies the corresponding real example: `FT_NONE`, `FT_STRING` and `FT_UINT_BYTES` do not meet the bit-insertion API's expectations; the accepted dissection uses appropriate compatible field types or another tree-add API.

**Checker rule:** validation must be API-specific: compare declared `hf` type and mask width against the exact tree insertion function and its value/storage semantics. A field that is legitimate for one tree-add API may be invalid for another. Keep source-checker coverage in step with new API families and test false positives before treating the check as blocking.

**Confidence:** Very high: two merged master fixes, including a checker update authored and merged by Martin Mathieson.
