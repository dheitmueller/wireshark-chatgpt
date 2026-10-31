# Wireshark automated MR review — !9812 through !9763

Review date: 2026-10-30
Corpus repository: `dheitmueller/wireshark-corpus-mrs`
**Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `671c79332799a008342626ba1e717e8548cb6a65`
Direction: highest-numbered previously unreviewed MR first.

**Exactly 50 distinct MR records, in descending order: 48 merged, one open (!9799), one closed without merging (!9778).**

## Selection and duplicate-review controls

Compared the corpus and notebook heads with existing review tracking, including the aggregate `reviewed-mrs.md`, supplemental `reviewed-mrs-automation.md`, the 350-file per-run directory inventory, exceptional/noncontiguous/gap ledgers, and recent per-run exact membership tables. The preceding review reconciled the prior frontier and identified !9812 as the next candidate. No tracking table or exception file marked any of !9812–!9763 reviewed, and all fifty records exist in this unchanged corpus. Filename ranges alone were not interpreted as exact reviewed membership.

The historical `reviewed-mrs-automation-17571-17620.md` was checked and **preserves 50 distinct explicitly listed entries from !17571 through !17620**. It was left unchanged.

## Exact reviewed-MR membership

| MR | Corpus outcome | Depth | Evidence and review note |
|---|---|---|---|
| !9812 | merged | Scanned | John Thacker stable TCP one-more-segment state: clear only after new data; reinforces !9818. |
| !9811 | merged | Scanned | NET_DM payload inference when kernel omits type; add REASON attribute. |
| !9810 | merged | Scanned | John Thacker rejects single nonprintable SIP/TCP keepalive rather than fabricating continuation. |
| !9809 | merged | Deep | John Thacker distinguishes retransmission on wire from bytes consumed by out-of-order reassembler; tests. |
| !9808 | merged | Scanned | TCP sequence-analysis preference-off null guard (stable backport). |
| !9807 | merged | Scanned | Same TCP preference-off guard, another stable branch. |
| !9806 | merged | Scanned | Same TCP preference-off guard, another stable branch. |
| !9805 | merged | Scanned | Freedesktop metainfo screenshot URL repair after documentation move. |
| !9804 | merged | Scanned | Same metainfo URL repair, branch variant. |
| !9803 | merged | Scanned | JDWP Java 19 command-set registration update. |
| !9802 | merged | Scanned | Same metainfo URL repair, branch variant. |
| !9801 | merged | Deep | John Thacker implements repeatable/mixed -j/-J selectors with per-protocol flags in shared output APIs. |
| !9800 | merged | Scanned | O-RAN FH CUS section extensions 7,8,9,14,15. |
| !9799 | opened | Discussion-focused | Unmerged -X validation proposal; Gilbert Ramirez recommends factoring duplicated CLI listing into wsutil. |
| !9798 | merged | Deep | John Thacker pinpoints 4-/6-byte fixed-header decrements before RDMnet list loops; sample capture. |
| !9797 | merged | Scanned | PFCP TEBUR label typo only. |
| !9796 | merged | Scanned | Remove unreachable RTPS remaining-items branch. |
| !9795 | merged | Deep | USBLL STALL completes transfers; track acknowledgement and logical URB direction. |
| !9794 | merged | Discussion-focused | João Valverde defines deliberate MSYS2 target-platform semantics for Windows ARM64. |
| !9793 | merged | Deep | Relocatable ELF privilege-safe dumpcap via static wsutil variant; release notes. |
| !9792 | merged | Scanned | Revert previous relocation attempt after MSYS2 regression. |
| !9791 | merged | Scanned | Only probe x86 SSE4.2 compiler flags for x86 targets. |
| !9790 | merged | Scanned | PROFINET fixes duplicate filter abbreviations with incompatible types. |
| !9789 | merged | Deep | Martin Mathieson adds bitmask-array width consistency checker and repairs registrations. |
| !9788 | merged | Scanned | Automated manufacturers, translations and identifiers refresh. |
| !9787 | merged | Scanned | Automatic data update on release branch. |
| !9786 | merged | Scanned | Automatic manufacturer/author data; services updater failure noted. |
| !9785 | merged | Scanned | Remove redundant wmem CMake object sublibrary. |
| !9784 | merged | Scanned | Docbook guide-source/image directory reorganization. |
| !9783 | merged | Scanned | Make RTPS-local lookup helper static. |
| !9782 | merged | Scanned | Docbook source/image layout changes, branch variant. |
| !9781 | merged | Scanned | Docbook source/image layout changes, branch variant. |
| !9780 | merged | Deep | João Valverde moves shared cmdarg_err below UI to wsutil, eliminating dumpcap UI dependency. |
| !9779 | merged | Scanned | Bluetooth 5.4 feature dissection; minor formatting review. |
| !9778 | closed | Discussion-focused | Superseded sshdump privilege selector; John Thacker cites already merged !9824; not implementation evidence. |
| !9777 | merged | Scanned | PFCP Association Release Request URSS flag. |
| !9776 | merged | Deep | RDP channel/conversation string lifetime and dynamic-channel reassembly/decompression. |
| !9775 | merged | Scanned | John Thacker Qt scrollbar click mapping includes pageStep (backport). |
| !9774 | merged | Scanned | Same Qt scrollbar mapping correction, branch variant. |
| !9773 | merged | Scanned | Same Qt scrollbar mapping correction, branch variant. |
| !9772 | merged | Deep | John Thacker UTF-8-safe DICOM truncation, retain complete error comment. |
| !9771 | merged | Deep | Guy Harris renames raw TLS supported-group values for actual protocol meaning, not JA3 use. |
| !9770 | merged | Deep | Gerald Combs XML 1.0 prohibited controls rendered as textual escapes; preserve UTF-8. |
| !9769 | merged | Deep | Gerald Combs fixes XML streaming escape buffer overflow with maximum expansion reserve. |
| !9768 | merged | Scanned | macOS auxiliary installer package versions centralized in CMake. |
| !9767 | merged | Scanned | Same macOS auxiliary packaging version changes, branch variant. |
| !9766 | merged | Scanned | Spelling fixes including RTPS filter abbreviation. |
| !9765 | merged | Deep | RPL/RFC bit positions corrected MSB-first; author traces earlier ND-EARO mistake. |
| !9764 | merged | Scanned | Sharkd RGB CSS hexadecimal strings zero-pad to six places. |
| !9763 | merged | Deep | Sharkd status returns optional active column titles with updated request/response tests. |

## Promoted durable conventions

- TCP application reassembly must distinguish sender-level retransmissions from bytes already passed to dissectors (!9809; John Thacker), and must not equate another segment with actual progress (!9812).
- Packet container length budgets must account for fixed headers before variable lists (!9798; direct John Thacker review).
- Terminal STALL/ACK protocol state can determine logical USB URB completion and direction (!9795).
- All fields in a bitmask array must have consistent declared containing widths; enforce this mechanically (!9789; Martin Mathieson).
- A relocatable privileged helper cannot assume relative dynamic-loader search paths work; review static/shared linkage and CI portability together (!9793, !9792; João Valverde).
- Repeatable `-j`/`-J` output selectors need independent semantics and safe recursive filter state (!9801; John Thacker).
- Shared diagnostics must not drag UI linkage into dumpcap (!9780; João Valverde); decoded variables should be named after their protocol meaning (!9771; Guy Harris).
- XML emitters must respect invalid XML 1.0 control characters and reserve buffer space for worst-case expansion (!9770 and !9769; Gerald Combs).
- UTF-8 truncation, protocol bit-order diagrams, conversation-retained allocator scope and sharkd JSON response tests require explicit semantics (!9772, !9765, !9776, !9763).

Open !9799 is design discussion only and closed/superseded !9778 is not evidence of accepted implementation. Routine automated data updates, backports, and duplicated Docbook moves were examined but did not warrant redundant new rules.

**Next expected candidate: !9762**, subject to rechecking for new corpus entries and explicit tracking before selection.
