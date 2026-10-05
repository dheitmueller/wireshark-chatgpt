# Review findings — Wireshark MRs !10312 through !10300

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## !10312 — DRDA: Support SQLATTR

Merged master work by John Thacker. SQLATTR carries the same FD:OCA SQLSTTGRP data object as SQLSTT, so the accepted handoff creates one `sqlstt_handle` and registers it for both code points. Durable lesson: when multiple dissector-table keys have the same decode contract, reuse the same handle instead of creating duplicate handles for the same parser.

## !10311 / !10310 / !10308 — GitLab CI: Force the installation of llvm-15

!10308 is Gerald Combs's merged master change; !10310 and !10311 carry it to release-4.0 and release-3.6. A packaging collision between Ubuntu jammy-updates and upstream LLVM packages is worked around with dpkg `--force-overwrite`, with comments linking both the Ubuntu and LLVM issue trackers. Durable lesson: keep external-dependency workarounds narrow, document the upstream defect that justifies them, and propagate the same workaround consistently to supported branches when the dependency problem applies there.

## !10309 / !10306 — BFCP: Fix length for some attributes

Merged release backports of the BFCP attribute-length correction. For several non-grouped text attributes, the protocol length includes the one-byte type and one-byte length fields but excludes padding, so payload text length and offset advancement change from `length - 3` to `length - 2`. These backports add no new architectural lesson beyond the master fix, but reinforce that displayed field length and cursor advancement must derive from the same exact protocol length semantics.

## !10307 — CIP: make a function static

Merged master cleanup by Martin Mathieson. A helper with no cross-translation-unit contract is made `static`. Useful C linkage hygiene, but too general to warrant a separate Wireshark-specific convention.

## !10305 — DRDA: Use dissector_try_uint_new

Merged master work by John Thacker. Nested DRDA command and parameter dispatch changes from `dissector_try_uint()` to `dissector_try_uint_new(..., FALSE, NULL)` because the nested dispatch should not repeatedly add DRDA to the frame's protocol list. Durable lesson: dissector-table dispatch APIs have presentation/layering side effects; use the variant whose side effects match the logical protocol layering rather than treating all lookup helpers as interchangeable.

## !10304 / !10303 / !10302 — ISAKMP: fix dissection of DEVICE_IDENTITY identity type

!10302 is Pascal Quantin's merged master fix; !10303 and !10304 are release backports. The two-bit identity type occupies the two least-significant bits, but its bit extraction still requires `ENC_BIG_ENDIAN`. Bit significance and byte/bit ordering are independent concepts. This strongly corroborates the existing `wire-encoding-conventions.md` guidance rather than establishing a new rule.

## !10301 — nhrp: various fixes

Merged master work authored by Guy Harris and the highest-authority evidence in this batch. The refactor gives `dissect_nhrp_mand()` a subset TVB containing exactly the Mandatory Part instead of the full NHRP TVB plus caller-managed offset/end arguments. The helper starts at local offset zero and takes its end from `tvb_reported_length()`. This directly expresses the protocol region as a bounded TVB and prevents a helper from accidentally parsing extensions as mandatory data.

The same change replaces a set of derived Booleans (`isReq`, `isErr`, `isInd`) with direct `switch` statements on the NHRP operation type, which keeps packet-layout decisions tied to the authoritative discriminator. It also attaches extension-offset diagnostics to the extension-offset field item itself, adds the Traffic Code field for Traffic Indication packets, and gives the registered extension-type field its value table so ordinary tree rendering owns the interpretation.

Durable lesson: when a helper owns one length-delimited semantic substructure, prefer a subset TVB whose domain is exactly that substructure. The helper can then use local offsets and TVB length semantics instead of duplicating parent-boundary bookkeeping.

## !10300 — Qt+Docs: I/O Graph updates

Merged master work by Gerald Combs. Checkbox labels are changed to sentence case, and the User's Guide is updated in the same change to document Automatic updates and Enable legend. This is good UI/documentation synchronization evidence but is not promoted as a standalone engineering rule.

## Discussion weighting

The corpus snapshots for all 13 MRs contain no substantive non-system review comments. Accordingly, this run does not invent review guidance from approval events. Merged master diffs and authorship are weighted most heavily, with Guy Harris's !10301, John Thacker's !10305/!10312, Pascal Quantin's !10302, and Gerald Combs's !10308 providing the strongest durable evidence. Stable-branch backports are treated as corroboration, not independent architectural authority.

## ST 291 / VANC check

No SMPTE ST 291/VANC packet type or related dissector work appears in this batch.
