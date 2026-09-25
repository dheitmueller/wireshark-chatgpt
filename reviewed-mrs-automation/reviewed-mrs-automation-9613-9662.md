# Automated MR review ledger: !9613–!9662

Reviewed 2026-09-25 using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed in this run. Candidate membership was checked against the accumulated automation tracking and `reviewed-mrs.md`; the immediately preceding exact ledger was also checked, and the historical !17571–!17620 ledger was revalidated as 50 unique MRs.

| MR | Outcome | Depth | Notes |
|---|---|---|---|
| !9662 | merged | Deep | Native macOS merge-request CI job; follow-up review exposed missing required SpeexDSP setup and led to !9678/!9679. |
| !9661 | merged | Scanned | RTPS PING string dissection; narrow protocol addition. |
| !9660 | merged | Scanned | NGAP NAS-PDU regression fix updates ASN.1 configuration and generated output together. |
| !9659 | merged | Scanned | NAS 5GS network-feature-support IE correction. |
| !9658 | closed / superseded | Deep | Rich review: Decode-As-safe `match_uint`, Expert Info instead of console warnings, programmer invariant assertion, capture/release-note/naming/submission guidance; resubmitted and merged as !10124. |
| !9657 | merged | Scanned | Makes translation-unit-local symbols static. |
| !9656 | merged | Scanned | Logray Qt 6.4 compilation fix mirrored from Wireshark. |
| !9655 | merged | Deep | Broad typed-item cleanup; John Thacker points out one affected source is generated from an ASN.1 template. |
| !9654 | merged | Deep | RTPS DomainId now prefers discovery state keyed by participant GUID over port-derived inference. |
| !9653 | merged | Deep | CMake link-what-you-use cleanup; John Thacker review protects plugin/public dependency usability and PUBLIC/PRIVATE target semantics. |
| !9652 | merged | Scanned | Registers TLS in the HTTP Upgrade dissector table. |
| !9651 | merged | Scanned | MSYS2 README update. |
| !9650 | merged | Scanned | Stable-branch lower-case documentation filename fix. |
| !9649 | merged | Scanned | Stable-branch lower-case documentation filename fix. |
| !9648 | merged | Scanned | Master lower-case documentation filename fix. |
| !9647 | merged | Scanned | Reverts Linux rpath/relocation change after regression. |
| !9646 | merged | Scanned | Stable documentation filename preparation. |
| !9645 | merged | Scanned | Stable documentation filename preparation. |
| !9644 | merged | Scanned | macOS package-image fix/backport: copy top-level packages and use recommended DMG format. |
| !9643 | merged | Scanned | macOS package-image fix/backport. |
| !9642 | merged | Scanned | Backport correcting CAG-only flag descriptions. |
| !9641 | merged | Scanned | Backport correcting CAG-only flag descriptions. |
| !9640 | merged | Scanned | Master correction to CAG-only flag descriptions against 3GPP TS 24.501. |
| !9639 | merged | Scanned | Comment typo correction aligned with final RFC text. |
| !9638 | merged | Deep | John Thacker RFC 7983 design: deterministic STUN/TURN/ZRTP/DTLS/RTP multiplex dispatch instead of heuristic side effects. |
| !9637 | merged | Deep | `value_string` duplicate detected by runtime/check pipeline; corrected against specification. |
| !9636 | merged | Deep | Fixes real typed-item issues and suppresses checker cases that are intentionally valid. |
| !9635 | merged | Scanned | macOS code-signing identifiers gain project prefix. |
| !9634 | merged | Scanned | ChmodBPF installer moves from deprecated `launchctl load` to `bootstrap system`. |
| !9633 | merged | Scanned | Documentation source-file naming cleanup. |
| !9632 | merged | Scanned | MSYS2 setup maintenance. |
| !9631 | merged | Deep | Fixes protocol-tree item lengths that were shorter than their registered/decoded values. |
| !9630 | merged | Scanned | macOS package-image fix on master. |
| !9629 | merged | Deep | `check_typed_item_calls.py` gains narrow regex exceptions for known-valid repeated filters while genuine duplicate filter names are fixed. |
| !9628 | merged | Scanned | Automated registry/translation data update. |
| !9627 | merged | Scanned | Automated registry/translation data update. |
| !9626 | merged | Scanned | Automated registry/translation data update. |
| !9625 | merged | Deep | Extcap splits “has options” from “requires user configuration”; review also catches a boolean-control presentation regression. |
| !9624 | merged | Scanned | Allows column caching during color dissection when it will not evict existing cache entries. |
| !9623 | merged | Scanned | Removes MSYS2 dependency packages after upstream availability. |
| !9622 | merged | Deep | Dependent-frame set moves from list to hash table for large reassemblies; John Thacker catches NULL-table iteration edge case. |
| !9621 | merged | Deep | Resource lookup uses actual application-bundle mode and consistent safe DATA_DIR/PLUGIN_DIR override precedence. |
| !9620 | merged | Deep | Extends VITA 49 CIF1 fields; review/checker catches API annotation and FT_BOOLEAN width issues. |
| !9619 | merged | Deep | RDP decoding/decompression fixes plus static-analysis cleanup; review explicitly considers stable backport scope. |
| !9618 | merged | Scanned | Adds NFSv4.1 CB_RECALL_ANY support. |
| !9617 | merged | Deep | Dissector generator gains plugin mode and emits/registers the required plugin CMake skeleton. |
| !9616 | merged | Deep | HTTP replaces ambient packet-scope calls with explicit allocator context and removes global per-packet stats state. |
| !9615 | merged | Deep | ICMPv6 EARO support; Alexis La Goutte requests a representative capture and contributor supplies encrypted sample plus key. |
| !9614 | merged | Deep | Couchbase snapshot flags; Alexis requests a representative capture and contributor offers one on the MR. |
| !9613 | merged | Scanned | Guy Harris man-page terminology cleanup; documentation-only, no durable new review convention promoted. |

Exact reviewed set:

`!9662, !9661, !9660, !9659, !9658, !9657, !9656, !9655, !9654, !9653, !9652, !9651, !9650, !9649, !9648, !9647, !9646, !9645, !9644, !9643, !9642, !9641, !9640, !9639, !9638, !9637, !9636, !9635, !9634, !9633, !9632, !9631, !9630, !9629, !9628, !9627, !9626, !9625, !9624, !9623, !9622, !9621, !9620, !9619, !9618, !9617, !9616, !9615, !9614, !9613`
