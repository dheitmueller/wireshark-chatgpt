# Wireshark MR automated review: !8811–!8762

Reviewed on 2026-10-31 using GPT-6. Corpus repository: `dheitmueller/wireshark-corpus-mrs`.
**Pinned corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`.
**Notebook base commit:** `488d48a3e12d5a41e5be0d82fa5614a2432facac`.

**Exact reviewed membership (50 MRs, descending):** !8811, !8810, !8809, !8808, !8807, !8806, !8805, !8804, !8803, !8802, !8801, !8800, !8799, !8798, !8797, !8796, !8795, !8794, !8793, !8792, !8791, !8790, !8789, !8788, !8787, !8786, !8785, !8784, !8783, !8782, !8781, !8780, !8779, !8778, !8777, !8776, !8775, !8774, !8773, !8772, !8771, !8770, !8769, !8768, !8767, !8766, !8765, !8764, !8763, !8762.

**Outcome tally:** 47 merged; 2 closed without merging (!8803 and !8768); 1 open (!8789). Each record's stored discussion and code diff was consulted. The depth field distinguishes focused code review, discussion review and a scan of routine changes.

## Selection, deduplication and preserved history

The notebook contained 370 pre-existing entries in `reviewed-mrs-automation/` before this run. Their directory inventory (including special exact-list, gap, backfill and noncontiguous filenames), `reviewed-mrs.md`, the supplemental `reviewed-mrs-automation.md`, the latest exact-membership ledger, and the previous run's full 369-file reconciliation were consulted. The notebook was unchanged since that previous reconciliation. The lowest ordinary ledger begins at !8812, no exception overlaps the new candidate range, and the old ledger mentions !8811 only as a future candidate rather than a completed review. Selection was based on membership evidence, not a presumption that every number inside an arbitrary file range was reviewed. All 50 selected JSON records exist at the pinned corpus SHA. The historical !17571–!17620 ledger is preserved, containing **50 unique MR table rows**, with its original Git blob SHA `9393377c3adeb3a38d502532839765e3abe771fe` unchanged.

## Strongest reusable findings and evidence weighting

Merged implementation was weighted above closed and open candidates; backports or multiple versions of the same patch were not counted as independent design precedents. Particularly substantive maintainer feedback came from **Guy Harris**, **João Valverde**, **John Thacker**, **Gerald Combs**, **Anders Broman**, **Stig Bjørlykke** and **Jaap Keuter**. The key durable lessons are named dissector registration and handle reuse (!8809), X.691 unconstrained UTF8String lengths (!8808), Windows CI dependency/environment diagnosis instead of pinning TLS without cause (!8807), EK duplicate-field grouping with non-deterministic hash traversal caution (!8802), efficient UTF-8 validation and nullable optional outputs (!8792/!8799), packet/document charset fidelity (!8784/!8790/!8770), per-layer RTP padding context for EVS (!8767), and deliberate synchronization of generators and output (!8777–!8782).

**Qualified/unmerged evidence:** !8789's packet-limit proposal remains open: Guy Harris and Stig Bjørlykke's units/visibility comments are strong guidance but the implementation is unaccepted. Closed !8803 was withdrawn by its author and closed !8768 was superseded. !8763's filter-abbreviation spelling changes have an inferred profile-compatibility risk, not a reproduced regression. !8802's `g_hash_table_foreach()` traversal may change output order; its merged status does not establish deterministic serialization.

## Exact MR assessments

| MR | Outcome | Depth | Assessment |
| --- | --- | --- | --- |
| !8811 | merged | discussion | Gerald Combs removed numerous unused assignments reported by fuzz builds but missed by a separate code-check job; CI configurations must be compared rather than assumed equivalent. |
| !8810 | merged | scanned | BPDU removes an unused MSTI counter, without changing decoded output. |
| !8809 | merged | deep | Named register_dissector() handles in 19 dissectors enable rawshark/fuzzshark and Lua lookup; Anders Broman preferred reusing the returned handle instead of repeated find_dissector(). |
| !8808 | merged | deep | PER UTF8String uses an unconstrained length in octets per X.691 §27.6, an aligned value TVBuff, and a separate zero-length path. |
| !8807 | merged | deep | Windows GitHub Actions Chocolatey workflow; Gerald Combs and Chuck Craft traced dependency-host/catalog/CMake issues, and a speculative forced TLS 1.2 override was removed. |
| !8806 | merged | scanned | IEEE 802.11 measurement request/report enumeration additions and corrected Measurement Pause request code. |
| !8805 | merged | discussion | TShark elastic-mapping skips default string mappings; documentation updated to describe the output contract. |
| !8804 | merged | scanned | Capture wireless utilities remove an unused wiphy band index. |
| !8803 | closed | discussion | John Thacker withdrew a vertical-tab whitespace helper after the malformed capture could not be reproduced and master appeared to handle it better; not an accepted API. |
| !8802 | merged | deep | EK JSON repeated-field grouping by field abbreviation and refreshed DHCP baselines; g_hash_table_foreach has no guaranteed stable key order. |
| !8801 | merged | scanned | Clang 15 GitLab CI change cherry-picked to a release branch. |
| !8800 | merged | scanned | Separate branch backport of GitLab CI Clang 15 configuration; not an independent precedent. |
| !8799 | merged | deep | wmem_strbuf_utf8_validate accepts a NULL end-position output and documents embedded NUL handling; follow-up to João/John review in !8792. |
| !8798 | merged | scanned | Primary GitLab CI Clang 15 update, corroborated by backports !8800 and !8801. |
| !8797 | merged | scanned | Release backport of !8793: IPv6 embedded IPv4 bit extraction uses the enclosing byte offset. |
| !8796 | merged | scanned | Periodic generated registries/ASTerix/translation data update; no distinct code-review convention. |
| !8795 | merged | scanned | Release-specific periodic generated source and translation update. |
| !8794 | merged | scanned | Another release-specific periodic source/translation update, not independent design evidence. |
| !8793 | merged | deep | IPv6 well-known-prefix nested parsing corrects an absolute 96-bit read to (offset*8)+96; preserves TVBuff origin. |
| !8792 | merged | deep | AFS RXStringV validates UTF-8 first and sanitizes only on failure; João Valverde requested the common-case efficient path; John Thacker requested nullable validator output. |
| !8791 | merged | scanned | HL7 message-type bytes are decoded by ASCII-aware TVBuff helper instead of raw byte copies. |
| !8790 | merged | deep | MMS text values decode with the specified ASCII/charset encoding instead of raw copies of wire bytes. |
| !8789 | opened | deep | Unmerged partial-capture loading proposal: Guy Harris distinguished packet ordinal from byte offset; Stig Bjørlykke requested explicit names and visible partial-data UI state. Do not treat as accepted. |
| !8788 | merged | discussion | Qt packet-list PageUp/PageDown preserves horizontal-scroll position; Roland Knall approved. |
| !8787 | merged | scanned | SCTP recognizes Zero Checksum Acceptable parameter; protocol-specific extension. |
| !8786 | merged | scanned | Qt deprecated parentWidget() replaced by qobject_cast<QWidget*>(parent()). |
| !8785 | merged | discussion | E2AP generated ASN.1 conformance uses encoding-aware string extraction for RAN function names; NULL length out-parameter accepted. |
| !8784 | merged | deep | WBXML inline and string-table values decode according to document charset persisted in packet protocol data, not presumed UTF-8 or raw pointer copying. |
| !8783 | merged | discussion | Jaap Keuter requested IPv6 'Hop Limit' rather than IPv4 TTL terminology in updated coloring rules. |
| !8782 | merged | deep | Skinny displayLabel replaces invalid high-bit ASCII/codebook bytes and synchronizes the generated source template. |
| !8781 | merged | scanned | Skinny generator XML/template re-sync backport, same implementation sequence as !8780. |
| !8780 | merged | scanned | Skinny template, XML and generated code re-sync after manual edits and generator indentation issue. |
| !8779 | merged | scanned | Python 3 Skinny generator transition release backport. |
| !8778 | merged | scanned | Additional Python 3 Skinny generator backport. |
| !8777 | merged | discussion | Primary Skinny Python 3 generator migration deliberately separated from regenerated dissector changes for backportability. |
| !8776 | merged | deep | CMake preserves explicit CMAKE_INSTALL_RPATH on macOS for executables, libraries and extcaps without changing empty defaults. |
| !8775 | merged | scanned | Accepted documentation terminology correction: acknowledgment. |
| !8774 | merged | deep | Display-filter Flex lexer accepts carriage returns as whitespace; regression exercises CRLF syntax. |
| !8773 | merged | scanned | Windows NSIS makes etwdump extcap an installed-by-default feature. |
| !8772 | merged | scanned | Release backport of DNS UAT appended-field default ports. |
| !8771 | merged | discussion | DNS resolution UAT uses uat_set_default_values() for both port 53 fields to read older persisted rows compatibly. |
| !8770 | merged | deep | XCSL token parser uses shared delimiter search and charset-aware string extraction rather than manual raw ASCII byte assembly. |
| !8769 | merged | scanned | User Guide documents display-filter layer operator (#), matching man-page section hierarchy. |
| !8768 | closed | scanned | Superseded typographical documentation attempt; corrected spelling merged via !8775. |
| !8767 | merged | deep | EVS format heuristic consumes packet-scoped RTP padding metadata keyed by enclosing layer; child TVBuff alone lacks framing context. |
| !8766 | merged | scanned | DBus helper linkage narrowed from external to static. |
| !8765 | merged | discussion | AllJoyn header type code uses FT_CHAR and value_string rather than invalid UTF-8-prone ad hoc formatting. |
| !8764 | merged | deep | construct_match_selected_string uses the typed fvalue formatter with display-base semantics; João Valverde approved. |
| !8763 | merged | discussion | Spelling cleanup also changes btatt.fragmet.* registered filter abbreviations; check saved-filter compatibility, not only prose. |
| !8762 | merged | discussion | E2AP new RIC Control IEs and callbacks added through ASN.1 conformance, template and generated dissector; check model-specific coverage. |

## Verification and next candidate

Ledger generation validated exactly 50 distinct descending MR numbers and a 47/2/1 status split. The corpus pinned commit is unchanged. The source confirms **!8761** exists and is the next expected candidate, subject to the next run's full duplicate-membership check. This review only examined stored corpus records and notebook source; no Wireshark builds, packet-capture execution, functional tests or fuzzing were performed.
