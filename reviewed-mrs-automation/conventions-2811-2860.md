# Convention synthesis — Wireshark MRs !2811–!2860

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Wiretap packet-option modeling — !2823 and !2859

Closed !2823 attempted to introduce a separate general TLV list for packet comments/options. Guy Harris pointed to the existing `wtap_block` option machinery. The contributor closed it, and merged !2859 moved packet comments/verdicts toward the existing block-option model. Guy further stated that packet options should live in the option array rather than duplicated dedicated packet-header fields.

**Rule:** extend the existing generic option abstraction when metadata already belongs to that domain. Do not introduce a second generic container for the same semantic class, and avoid duplicate special-case members once the generic representation is authoritative.

**Weight:** extremely high: direct Guy Harris review plus merged successor.

## Timestamp width — !2854

Guy Harris removed `guint32` casts from assignments to `nstime_t.secs`; that member is `time_t` and can be wider than 32 bits.

**Rule:** preserve the destination timestamp type's full range throughout conversion. Do not insert a 32-bit cast merely because historical builds used 32-bit time values.

**Weight:** extremely high: merged Guy Harris-authored master fix.

## Packed flags — !2841

Guy Harris changed ETW direction handling to use named `PACK_FLAGS_DIRECTION_*` constants and to interpret `pack_flags & PACK_FLAGS_DIRECTION_MASK`.

**Rule:** a packed flags word is not an enum. Isolate the semantic subfield before comparing it to enum constants.

**Weight:** extremely high: merged Guy Harris-authored master fix.

## Wire type versus presentation — !2822

Anders Broman rejected changing a PFCP OCTET STRING into a string merely because the bytes are usually printable. The accepted field remains `FT_BYTES` with `BASE_SHOW_ASCII_PRINTABLE`.

**Rule:** register the semantic wire type and use display metadata for a friendly representation.

**Weight:** very high: direct maintainer review incorporated before merge.

## Declarative mappings — !2834

Martin Mathieson requested `value_string` + `VALS()` for fixed one-to-one numeric mappings instead of `CF_FUNC`.

**Rule:** static numeric-to-label mappings belong in field metadata; use custom formatting only for presentation that actually requires computation.

**Weight:** high: direct review on a merged MR.

## Supported subtree APIs — !2831

Anders Broman requested `proto_tree_add_subtree()` / `proto_tree_add_subtree_format()`; accepted RTPS code follows that direction.

**Rule:** construct structural grouping nodes with supported subtree APIs rather than ad-hoc textual tree items where a structured API exists.

**Weight:** very high: direct maintainer review incorporated before merge.

## Tooling portability — !2844 and !2853

Guy Harris replaced non-portable `[[ ... ]]` use in a script expected to run under `/bin/sh`, skipped a Windows-only translation unit when running clang-check on Unix, and immediately corrected basename selection in !2853.

**Rule:** repository tooling must use the language promised by its interpreter and respect platform-conditional buildability. Exercise the tooling path after changing source-selection logic.

**Weight:** extremely high: merged Guy Harris-authored changes.

## Fixed string fallbacks — !2855

NVMe call sites whose fallback is a literal changed from `val_to_str()` to `val_to_str_const()`.

**Rule:** use the constant-return lookup variant when no formatting is required; do not allocate packet-scope strings merely to return a literal fallback.

**Weight:** high: merged Pascal Quantin cleanup.

## Encoded versus displayed precision — !2836

Pascal Quantin reduced time display precision to match the protocol's 100-microsecond encoding resolution.

**Rule:** display precision should not imply finer measurement precision than the wire representation can encode.

**Weight:** high: direct review incorporated before merge.

## Submission scope — !2816 → !2817

Closed !2816 unintentionally carried an entire prior feature while claiming to be a one-line compilation fix. Guy Harris called out the mismatch; the contributor closed it and submitted focused merged !2817.

**Rule:** verify that the actual branch diff matches the MR title and intent. If branch history pulls in unrelated work, rebuild the focused submission.

**Weight:** moderate-to-high negative-plus-successor evidence.
