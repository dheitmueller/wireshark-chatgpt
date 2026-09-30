# Review findings: Wireshark MRs !3661–!3710

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is treated as accepted precedent. Closed MRs are retained only as lower-weight supersession/review history. Maintainer-authored changes and direct maintainer review, especially Guy Harris, receive greater weight.

| MR | State | Author | Review finding |
|---|---|---|---|
| !3710 | merged | Evan Huus | Broad accepted migration from ambient `wmem_packet_scope()` to explicit `pinfo->pool`; documentation now says packet-info pool should be preferred when `pinfo` is available. This is the authoritative successor to closed !3695. |
| !3709 | merged | David Perry | Filter-tooltip wording cleanup. Anders Broman raised maintainability concerns about references tied to numbered documentation sections; low architectural weight. |
| !3708 | merged | David Perry | Moves packet drop count, packet ID, and interface queue from dedicated packet-header fields plus presence bits into typed `WTAP_BLOCK_PACKET` options. Option existence now carries presence semantics. |
| !3707 | merged | gtker | New WOWW dissector with sample captures. Alexis La Goutte requested removal of template residue and avoidance of the problematic `index` identifier; accepted after cleanup. |
| !3706 | merged | Evan Huus | Removes unused sharkd variables; routine cleanup, no new durable convention. |
| !3705 | merged | Gerald Combs | Removes obsolete `NEED_STRPTIME` define; build cleanup, no new durable convention. |
| !3704 | merged | Gerald Combs | BLF Win32 compilation fixes; reinforces cross-platform type/build validation but adds no distinct rule beyond existing portability guidance. |
| !3703 | merged | Gerald Combs | Automated release-3.2 registry/docs update; routine generated/release maintenance. |
| !3702 | merged | Gerald Combs | Automated release-3.4 registry/docs update; routine generated/release maintenance. |
| !3701 | merged | Gerald Combs | Automated master registry/docs/translation update; routine generated maintenance. |
| !3700 | merged | Evan Huus | Removes unused registered fields across dissectors; routine dead-code cleanup. |
| !3699 | merged | Guy Harris | High-authority ownership fix: `wtap_block_get_nth_string_option_value()` returns block-owned storage. ERF must duplicate a value before retaining/freeing it independently, avoiding dangling ownership/double-free. |
| !3698 | merged | Jaap Keuter | Master DLM3 TCP framing fix: multiple PDUs can share a TCP segment and one PDU can span segments; introduces `tcp_dissect_pdus()` with a length callback and per-PDU decoder. |
| !3697 | merged | Jaap Keuter | Release-3.4 backport of !3698; corroborates the TCP byte-stream framing rule but is secondary to the master-origin change. |
| !3696 | merged | Dr. Lars Völker | LIN ID parsing correction; focused protocol bug fix. |
| !3695 | closed | Evan Huus | Superseded first attempt at the broad `pinfo->pool` conversion; lower-weight history. Merged !3710 is authoritative. |
| !3694 | merged | Jaap Keuter | XML BOM handling fix; preserves rather than hides meaningful UTF-8 input bytes. Focused display/parsing correction. |
| !3693 | merged | Developer Alexander | JSON unescape buffer-overflow fix redesigned around bounded TVB access and `wmem_strbuf`. Gerald Combs verified crash reproducers under allocator-debug tooling and rejected changing field-value semantics merely to accommodate the refactor; existing unquoted string behavior was restored. |
| !3692 | merged | Dr. Lars Völker | BLF clang-warning cleanup; static-analysis hygiene, no new distinct convention. |
| !3691 | merged | Arkady Gilinsky | Adds OAMPDU network-port declaration parsing; protocol feature, no cross-cutting rule extracted. |
| !3690 | merged | Gerald Combs | Documentation CSS image-width fix; no durable code convention. |
| !3689 | merged | Dr. Lars Völker | LIN IDs are only unique within a bus. Adds bus-aware identity/mapping and an explicit bus-ID-zero wildcard fallback, reinforcing composite identity keys for bus-local identifiers. |
| !3688 | merged | Gerald Combs | Removes obsolete CMake probes; build cleanup. |
| !3687 | merged | Dr. Lars Völker | Adds a TECMP Channel-ID-name filter; protocol/UI enhancement, no distinct cross-cutting rule. |
| !3686 | merged | Ivan Nardi | Extends WSLua ProtoField masks to 64 bits. Because Lua numbers cannot exactly represent every 64-bit integer, accepted API also supports exact-width `UInt64` userdata and string conversion; tests cover number/string/userdata/nil/invalid cases and cross-platform compiler diagnostics. |
| !3685 | merged | Dr. Lars Völker | Signal-PDU adopts the new CAN dispatch API from !3668; downstream corroboration of the split standard/extended CAN ID domains. |
| !3684 | merged | Dr. Lars Völker | ISO15765 adopts the new CAN dispatch API from !3668; downstream corroboration. |
| !3683 | merged | Gerald Combs | NSIS DPI-awareness improvement; platform GUI packaging detail, no general notebook rule extracted. |
| !3682 | merged | Dr. Lars Völker | Propagates the new CAN dispatch API to other CAN packet sources; supports having one common semantic dispatch path across carriers. |
| !3681 | merged | Jaap Keuter | Corrects JUNIPER protocol-item source length; reinforces existing protocol-item range accuracy. |
| !3680 | merged | Developer Alexander | CAN column-info readability cleanup; presentation-only. |
| !3679 | merged | Arkady Gilinsky | Fixes OAMPDU GetRequest parsing so Object 0 does not prematurely terminate parsing; authoritative successor to closed !3677. |
| !3678 | merged | Martin Mathieson | O-RAN FH CUS C-section dissection correction; focused protocol fix. |
| !3677 | closed | Arkady Gilinsky | Superseded duplicate of !3679; lower-weight history only. |
| !3676 | merged | Guy Harris | High-authority follow-up for `--capture-comment`: option checks must not be hidden behind libpcap build guards when read-file→write-file operation still supports the feature, and output capability must be queried from the selected file type rather than hard-coding pcapng. |
| !3675 | merged | Guy Harris | High-authority CLI/architecture cleanup: capture comments do not belong in live-capture-only state because TShark can add them during file conversion; repeated options add multiple comments; docs say “add” rather than “set” and avoid assuming pcapng is the only format that can ever support comments. |
| !3674 | merged | Gerald Combs | Removes obsolete CMake checks for `fcntl.h` and `floorl`; routine baseline cleanup. |
| !3673 | merged | Evan Huus | TCP proof-of-concept for replacing ambient packet scope with `pinfo->pool`. Evan validated with Valgrind and GUI memory-use browsing before broader rollout in !3710. |
| !3672 | merged | Gerald Combs | CMake Qt include cleanup; no new durable rule. |
| !3671 | merged | Stefan Metzmacher | SMB3.1.1 AES-256 CCM/GCM support adds real capture fixtures and tests for session-key derivation, explicit decryption keys, partial captures, and swapped keys. Alexis La Goutte also required follow-up on a static-analyzer dead-store warning. |
| !3670 | merged | Gerald Combs | Version bump 3.2.15→3.2.16; release bookkeeping. |
| !3669 | merged | Gerald Combs | Version bump 3.4.7→3.4.8; release bookkeeping. |
| !3668 | merged | Developer Alexander | Splits CAN standard-ID and extended-ID subdissector tables because their numeric ranges overlap, gives the more-specific tables priority, and preserves the legacy generic table as fallback for compatibility. |
| !3667 | merged | Gerald Combs | Reduces GitLab CI test output; CI presentation/resource hygiene. |
| !3666 | merged | Gerald Combs | Builds Wireshark 3.2.15 release; release bookkeeping. |
| !3665 | merged | Gerald Combs | Builds Wireshark 3.4.7 release; release bookkeeping. |
| !3664 | merged | Jan Müller | DoIP TLS handover support; transport feature, no new cross-cutting convention extracted. |
| !3663 | merged | Gerald Combs | Corrects SpanDSP/TIFF include integration; dependency/build cleanup. |
| !3662 | merged | Alexis La Goutte | SV spelling correction is made in ASN.1 source and regenerated C together, consistent with the generated-source rule that semantic edits belong in the generator/template authority. |
| !3661 | merged | Martin Mathieson | Makes an ISO15765 helper file-local/static; reinforces existing symbol-visibility and implementation-locality guidance. |

## Strongest durable evidence

The highest-authority material in this batch is Guy Harris's !3699 ownership fix and Guy-authored !3675/!3676 capture-comment cleanup. The broad allocator migration (!3673 then !3710), typed packet-block metadata move (!3708), bus-scoped LIN identity (!3689), exact-width WSLua numeric handling (!3686), and CAN dispatch-domain split (!3668) are also strong merged architectural evidence.

No SMPTE 291/VANC packet type was encountered in this batch.
