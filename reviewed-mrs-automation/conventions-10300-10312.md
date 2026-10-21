# Convention synthesis — Wireshark MRs !10312 through !10300

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Bound semantic substructures with subset TVBuffs

Merged master MR !10301, authored by Guy Harris, refactors NHRP so the Mandatory Part helper receives a `tvb_new_subset_length()` covering exactly that protocol region. The helper then starts at local offset zero and derives its end from the subset TVB rather than receiving the full packet plus a separate caller-managed end boundary.

**Rule:** when a helper owns one length-delimited semantic substructure, prefer a subset TVB whose domain is exactly that substructure. This makes containment part of the data object, lets the helper use local offsets and normal TVBuff bounds semantics, and reduces the risk of consuming following siblings or extensions.

**Confidence:** Extremely high. Merged master architecture work authored by Guy Harris.

## Match dissector-table dispatch side effects to logical layering

Merged master MR !10305, authored by John Thacker, changes nested DRDA dispatch from `dissector_try_uint()` to `dissector_try_uint_new(..., FALSE, NULL)` because subdispatch inside DRDA should not repeatedly add the DRDA protocol name to the frame's protocol list.

**Rule:** dissector-table lookup helpers are not interchangeable when they differ in protocol-layer or presentation side effects. For nested dispatch within one logical protocol, choose the variant that preserves the intended frame protocol stack and tree behavior.

**Confidence:** Very high. Merged master change with the side-effect rationale stated explicitly by the author.

## Reuse one dissector handle for table keys with the same decode contract

Merged master MR !10312, authored by John Thacker, registers one `sqlstt_handle` for both DRDA SQLSTT and SQLATTR because both code points carry the same FD:OCA SQLSTTGRP object.

**Rule:** when multiple dissector-table keys intentionally share the same wire payload contract, reuse a single handle for the common parser rather than creating duplicate handles around the same function. This makes the shared contract explicit at registration time.

**Confidence:** High. Merged master implementation by John Thacker.

## Least-significant-bit placement does not imply little-endian encoding

Merged master MR !10302, authored by Pascal Quantin, changes a two-bit ISAKMP DEVICE_IDENTITY field from `ENC_LITTLE_ENDIAN` to `ENC_BIG_ENDIAN` even though the semantic type occupies the two least-significant bits. Stable-branch MRs !10303 and !10304 carry the same fix.

**Rule:** determine the encoding from the protocol's bit/byte ordering contract, independently from whether the semantic field occupies low or high bits of the decoded value.

**Confidence:** Very high, but this is corroboration of the existing `wire-encoding-conventions.md` rule rather than a new convention.

## Keep temporary dependency workarounds narrow and traceable

Merged master MR !10308, authored by Gerald Combs, adds a targeted dpkg `--force-overwrite` workaround for an LLVM/Ubuntu packaging collision and links the relevant Ubuntu and LLVM issues in the CI file. Merged release-4.0 and release-3.6 backports !10310 and !10311 carry the same workaround.

**Rule:** when CI needs a temporary external-dependency workaround, constrain it to the affected operation, document the upstream defect that justifies it, and apply it consistently to supported branches that share the same dependency failure.

**Confidence:** High. Merged master infrastructure fix plus two accepted release-branch backports.

## Additional corroboration

- !10306 and !10309 reinforce that a protocol length field's precise inclusion/exclusion semantics must drive both displayed payload length and cursor advancement.
- !10307 reinforces file-local C linkage for helpers that have no cross-translation-unit contract.
- !10300 keeps UI wording and user documentation synchronized and uses sentence case for checkbox labels; useful presentation evidence, but not promoted as a standalone broad convention.
