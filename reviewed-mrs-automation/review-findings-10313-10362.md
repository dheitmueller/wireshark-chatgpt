# Review findings: Wireshark MRs !10313 through !10362

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !10362 through !10313. Merged master work is weighted most heavily; stable backports are corroboration; closed work is used only for negative or process evidence.

## High-value findings

### Generated dissectors: fix the source of truth

Merged !10330, !10341, and !10343 initially changed generated output directly. Guy Harris explicitly required the corresponding ASN.1 template or conformance input to be changed and the output regenerated. The accepted follow-ups are !10353, !10351, and !10352. Guy's stable-branch MRs !10354-!10357 preserve the same source/output parity.

### Compiler-family detection must not confuse compatibility with identity

Merged !10329 improves compiler-specific diagnostic selection. John Thacker points out that `__GNUC__` may be defined by non-GCC compilers, that Intel Classic and Intel LLVM use different identity macros, and that Clang uses `__clang__`. The accepted code defines the GCC-only version macro only after excluding Clang, Intel Classic, and Intel LLVM. Merged !10318 is the precursor: João Valverde rejects an ad-hoc GCC major-version comparison and asks for the project version helper.

### Weak magic values remain heuristic evidence

Merged !10331 explains why MPEG wiretap remains registered as heuristic despite magic-like byte patterns: the signatures are short and prone to false positives. Merged !10328 complements that with an ordering rule: prefer more discriminating and faster heuristics before weaker or slower ones. !10327 updates extension hints without treating them as proof of identity.

### Binary semantics should be represented as Boolean fields

Merged !10359 replaces many two-entry integer value tables with `FT_BOOLEAN` plus shared `true_false_string` definitions. Merged !10347 does the same for MPEG/DVB current/next indicators. The Boolean container width still reflects the encoded carrier when a mask is used.

### Global lifecycle changes may be better than per-dissector compensation

Closed !10345 proposed broad SRT helper changes to compensate for UI time shifts. John Thacker said it was simpler to trigger redissection on a time shift; the issue was fixed that way and the MR was closed. Because the implementation did not merge, this is retained as negative architecture evidence rather than an implementation exemplar.

### Strengthen project checkers rather than working around blind spots

Merged !10315 teaches `check_typed_item_calls.py` to resolve macro-defined masks before validating widths and masks, then fixes the real field-registration defects exposed by the stronger check.

## Additional evidence

!10362 makes integer wmem-tree query operations tolerate a NULL tree as empty. !10361 adds H.264 Annex B bytestream parsing and guards short NALs. !10358 updates HI2Operations at ASN.1/configuration/generated layers. !10350 adds H.264 access-unit-delimiter parsing and incorporates an Alexis La Goutte typo correction. !10349, authored by Guy Harris, honors protocol ID -1 as a no-protocol sentinel. !10346 reinforces source-level ownership of generated formatting. !10342/!10323/!10322 improve MPEG framing and program-end handling. !10320 replaces the old boolean spelling with `ENC_NA` in ASN.1 templates. !10316 accepts zero-length Protobuf timestamps. !10313 and backport !10314 add NNTP NULL guards.

The six closed MRs are !10360, !10345, !10335, !10334, !10333, and !10324. !10360 is a draft WSML export proposal; !10335/!10334/!10333 are automatic updates that missed their merge window; !10324 was closed after existing packet-byte C/Rust array output was shown to satisfy the use case.

No SMPTE ST 291/VANC packet type was encountered.
