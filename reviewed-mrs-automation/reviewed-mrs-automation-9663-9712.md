# Automated Wireshark MR review — !9712 through !9663

Review date: 2026-10-30
Corpus: `dheitmueller/wireshark-corpus-mrs`
**Corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `93d216d6e401252fbba2bede6e8cd0cf0d1cd7f9`
Selection order: descending from highest-numbered previously unreviewed MR.

**Exactly 50 distinct records: 48 merged and two closed/unmerged (!9712 and !9708).**

## Duplicate-avoidance controls

Verified the GitHub heads (corpus unchanged). Inventoried and read **all 352 files** in `reviewed-mrs-automation/`, including exceptional, exact-membership, noncontiguous and gap ledgers; also read root `reviewed-mrs.md`. Searched explicit MR numbers in the complete tracking against this batch, rather than treating filenames or ranges as reviewed by implication. The only mention of a batch number was the preceding ledger's **next expected candidate** !9712, which was not a review. All fifty selected corpus JSON records were fetched and their available final diffs and retained non-system discussions examined. Merged master code and authoritative maintainer corrections carried most weight; stable backports and closed experiments received less. The oversized `file-pcapng.c` diff of !9688 is absent in the corpus, so the discussion and other retained patches inform that item's conclusion.

The existing `reviewed-mrs-automation-17571-17620.md` was separately checked for **50 explicit historical membership rows** spanning !17571–!17620 and is preserved untouched. No existing ledger was deleted, renamed or rewritten.

## Exact reviewed-MR membership

| MR | Outcome | Review depth | Evidence and finding |
| --- | --- | --- | --- |
| !9712 | closed | Discussion-focused | Abandoned macOS Homebrew package prefetch experiment; Gerald measured 9:03 vs 9:09 CI prerequisite time. |
| !9711 | merged | Deep | João centralizes UTF-8 repair in scoped wsutil helper; Gerald proposes API checker, João prefers advisory warning. |
| !9710 | merged | Scanned | John's XML 1.0 control-byte escape correction backported to release-4.0; master behavior previously covered. |
| !9709 | merged | Deep | CMake-generated HAVE_MSYSTEM replaces compiler-family heuristic for Windows install behavior. |
| !9708 | closed | Discussion-focused | Runtime MSYS2 detection abandoned by author as unnecessary with CPack; accepted alternative !9709. |
| !9707 | merged | Scanned | Remove obsolete wspcap.h wrapper; move compatibility definitions to generated configuration. |
| !9706 | merged | Scanned | Allow direct gcrypt.h includes; remove obsolete wsgcrypt.h restriction and warning suppression. |
| !9705 | merged | Deep | Use ws_utf8_make_valid for pcapng options, with Unicode semantics and John-tested Apple NUL-bearing sample. |
| !9704 | merged | Deep | Distinguish dissector handles from protocol names in UAT; Stig reports combo completion limitation, later !10148. |
| !9703 | merged | Scanned | Fix RDMnet and MSNLB field widths and narrow typed-item checker exceptions. |
| !9702 | merged | Discussion-focused | MySQL cloning command sample and Alexis's Windows CI rejection of GCC-only fallthrough attribute. |
| !9701 | merged | Deep | USBLL preserves first-pass global state when historical packet's state is restored for redissection. |
| !9700 | merged | Scanned | Automatic master ASTERIX, registries, OUI, localization and contributor data refresh. |
| !9699 | merged | Scanned | Automatic release-3.6 OUI/manuf update; maintenance duplicate. |
| !9698 | merged | Scanned | Automatic release-4.0 ASTERIX/registries/OUI refresh; maintenance duplicate. |
| !9697 | merged | Deep | John and Roland discuss minimum Qt baseline versus supported RHEL matrix; Qt 5.11 raise merged. |
| !9696 | merged | Scanned | release-3.6 backport of RTP packet-loss sorting fix from !9693. |
| !9695 | merged | Scanned | release-4.0 backport of RTP packet-loss sorting fix from !9693. |
| !9694 | merged | Deep | Alexis catches duplicate mysql.binlog.position abbreviation registered as incompatible UINT32/UINT64. |
| !9693 | merged | Deep | RTP loss sort computes the actual per-stream packet loss for both comparator operands. |
| !9692 | merged | Deep | CoAP post-merge Stig fixes: missing HF list, registered option semantics and sample attachment; successor !9731. |
| !9691 | merged | Scanned | Add registered CoAP content-format values. |
| !9690 | merged | Deep | John separates WTAP_ENCAP_NONE (no IDB yet) from UNKNOWN; removes TShark fake-IDB workaround. |
| !9689 | merged | Scanned | Improve TECMP lifecycle field presentation. |
| !9688 | merged | Deep | pcapng local block extension callbacks; Joakim drives pcapng.darwin namespace; oversized core diff omitted. |
| !9687 | merged | Scanned | release-3.6 ISO 15765/10681 fragment-counter hardening backport. |
| !9686 | merged | Scanned | release-4.0 ISO 15765/10681 fragment-counter hardening backport. |
| !9685 | merged | Deep | John checks merged output encapsulation against each input's supported types. |
| !9684 | merged | Scanned | Guy's TShark switch/break indentation correction backported to release-4.0. |
| !9683 | merged | Scanned | Guy's TShark switch/break indentation correction on master. |
| !9682 | merged | Scanned | Gerald parallelizes macOS GitLab CI jobs. |
| !9681 | merged | Scanned | Gerald scopes macOS MR CI job to supported upstream repository at the time. |
| !9680 | merged | Deep | ISO 15765/10681 reassembly fragment-index bound and high-fragment rollover guards. |
| !9679 | merged | Scanned | Homebrew setup shellenv and dependency installation option cleanup. |
| !9678 | merged | Scanned | Align GitHub macOS CI invocation with new brew --install-optional script option. |
| !9677 | merged | Deep | USB MSC BOT decoding workaround for USBLL reassembly limitations; command/status parser refactor. |
| !9676 | merged | Scanned | Guy's release-3.6 follow-up fixes filter argument passed into capture-filter hint helper. |
| !9675 | merged | Scanned | Guy's release-4.0 follow-up fixes filter argument passed into capture-filter hint helper. |
| !9674 | merged | Scanned | Guy's release-3.6 TShark BPF program and dead-pcap cleanup backport. |
| !9673 | merged | Deep | Guy fixes error-path helper use of dfilter instead of unrelated rfilter after !9670. |
| !9672 | merged | Scanned | Guy's release-4.0 TShark capture-filter hint memory-leak backport. |
| !9671 | merged | Scanned | ORAN FH CUS item-label wording and TODO cleanup. |
| !9670 | merged | Deep | Guy repairs TShark BPF program leak with pcap_freecode/pcap_close; refactors diagnostic helper. |
| !9669 | merged | Deep | John processes trailing pcapng IDBs after last packet in mergecap, editcap and TShark. |
| !9668 | merged | Scanned | release-3.6 GSM SMS empty-endpoint hash support backport. |
| !9667 | merged | Scanned | release-4.0 GSM SMS empty-endpoint hash support backport. |
| !9666 | merged | Deep | GSM SMS reassembly hash accepts missing/empty packet source/destination while requiring key data. |
| !9665 | merged | Scanned | Correct dissector spelling and filter-label typos; no new principle. |
| !9664 | merged | Scanned | release-4.0 Qt ShowAs enum/combobox ordering backport. |
| !9663 | merged | Deep | John aligns Qt Show Packet Bytes ShowAs enum ordering with combobox indexes. |

## Durable lessons promoted into the notebook

**Wiretap:** !9690 distinguishes yet-to-be-discovered interfaces from genuinely unknown encapsulation instead of faking IDBs; !9669 drains trailing IDBs before close; !9685 checks output encapsulation support for every merge input.

**Dissector and field APIs:** !9688 introduces typed pcapng local block handlers and an intentionally shared filter namespace; !9704 separates UAT dissector handles from protocols; !9694 bans incompatible duplicate field names/types; !9703 fixes width/decoder registration mismatches; !9692 requires fully initialized shared CoAP header-field lists even when a change merged early.

**State and robustness:** !9701 keeps USBLL's historical redissection state local instead of overwriting first-pass global state; !9680 guards both fragment-index bounds and sequence extension rollover; !9666 accepts valid empty endpoints in GSM SMS reassembly.

**Lifetime, encoding and CLI correctness:** !9711/!9705 centralize scope-aware Unicode sanitization; Guy Harris's !9670/!9673 repair the BPF diagnostic resource leak and wrong error-string argument.

**UI, submission and build:** !9663 fixes enum-to-combobox index coupling; !9693 sorts RTP streams by the real loss metric; !9697 resolves supported Qt baseline using actual platform evidence; !9702 corrects an MSVC-incompatible fallthrough annotation; !9692 emphasizes authoritative post-merge review and MR-hosted sample pcaps. Discarded !9712 and !9708 are recorded but not normative engineering precedents.

## Continuation

Next expected candidate: **!9662**, subject to a fresh corpus and exact-membership reconstruction. The corpus still contains older MRs. Leave the recurring review automation enabled and unpaused.
