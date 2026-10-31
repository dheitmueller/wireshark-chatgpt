# Wireshark automated MR review — !8911 through !8862

Reviewed **2026-10-31** with **GPT-6** against `dheitmueller/wireshark-corpus-mrs` at pinned corpus commit **`ddcaa22b51c68f594e425a23388c3a2086813054`**. Notebook base commit: `01c9a47cabfcda0698e6f7963f9cab3cf3ab84a6`.

**Exact reviewed membership (50 distinct MRs in descending order):** !8911, !8910, !8909, !8908, !8907, !8906, !8905, !8904, !8903, !8902, !8901, !8900, !8899, !8898, !8897, !8896, !8895, !8894, !8893, !8892, !8891, !8890, !8889, !8888, !8887, !8886, !8885, !8884, !8883, !8882, !8881, !8880, !8879, !8878, !8877, !8876, !8875, !8874, !8873, !8872, !8871, !8870, !8869, !8868, !8867, !8866, !8865, !8864, !8863, !8862.

**Snapshot outcomes:** 49 merged; one closed without merging (!8891, explicitly superseded by !17660). This is historical GitLab corpus state, not live status.

## Tracking and selection

The notebook's `reviewed-mrs-automation/` tree contained **368** existing entries: 350 ordinary range-named ledgers and 18 exceptional/supplemental files. Examined the latest exact-membership ledger, both aggregate ledgers, and all 16 exceptional ledger variants (including backfill, gap, noncontiguous, skipped and companion exact lists). The root ledger and supplemental aggregate contain no actual reviewed records below !8912; the 350 ordinary ledger filename starts are all at or above !8912. The latest prior exact ledger explicitly accounts for !8961–!8912. Using the previous run's earlier full reconciliation and actual per-run memberships rather than inferring that a numeric interval was reviewed simply from a few cited numbers, the next available unreviewed candidates are !8911–!8862. Every one of the 50 candidate `mr_<iid>.json` objects was verified to exist in the corpus; no candidate overlaps the earlier exact membership. Ledger cross-references to later repairs do not count those referenced MRs as new reviews.

The historical !17571–!17620 ledger remains **unchanged**, contains **50 distinct MR table entries**, and retains blob SHA `9393377c3adeb3a38d502532839765e3abe771fe`. No tracking files were modified except this new ledger.

## Extracted conventions and review weighting

Highest-confidence accepted observations include: John Thacker's HTTP2 UAT dynamic field lifecycle fix (!8901); Jaap Keuter and Alexis La Goutte's direction to use `proto_tree_add_checksum()` (!8907); the three ASN.1 28-/36-bit cell-ID masks (!8895–!8897); João Valverde and John Thacker's correction of a misleading standards claim in MIPv6 (!8884); John Thacker's packet byte-range versus text-value distinction (!8877); and the linked X509IF fixed-buffer and NULL-initialization fixes (!8863, !8868). Source-level cautions from merged !8910, !8900 and !8862 are explicitly separated from reviewer-endorsed design rules. The closed !8891 is down-weighted as superseded and not cited as accepted TCP behavior. Automated and repetitive cherry-pick changes were scanned and not multiplied into separate architectural precedents.

## Exact MR assessments

| MR | Snapshot outcome | Depth | Assessment |
| --- | --- | --- | --- |
| !8911 | merged | scanned | Windows release-3.6 GitHub CI temporary CMake 3.24.3 pin; branch duplicate of 8893/8906. |
| !8910 | merged | deep | Display-filter autocomplete uses parser prefix errors and DF_ERROR_UNEXPECTED_END; João suggested df_error_set_msg; conditional merits edge-case testing. |
| !8909 | merged | scanned | Qt log handler uses LOG_LEVEL_ECHO for debug and includes optional file/line/function context. |
| !8908 | merged | discussion | VHT beamforming expansion; Alexis found Clang Analyzer dead stores and prompted Werror discussion. |
| !8907 | merged | deep | MongoDB CRC32C OP_MSG checksum uses proto_tree_add_checksum after explicit Jaap/Alexis review. |
| !8906 | merged | scanned | Another Windows CI branch CMake 3.24.3 pin. |
| !8905 | merged | discussion | Qt6 RTP playback marker resume fix; Windows confirmation and 4.0 backport requested. |
| !8904 | merged | scanned | Automated registry/translations update; weak independent design evidence. |
| !8903 | merged | scanned | Automated manufacturer/service updates; weak independent design evidence. |
| !8902 | merged | scanned | Automated registry/service updates; weak independent design evidence. |
| !8901 | merged | deep | HTTP2 dynamic UAT header-field registration tied to post_update/reset, not each capture open/close; stale filter IDs and deferred ownership. |
| !8900 | merged | deep | Qt TCP override selection crash; John Thacker distinguished 1+ from multiselection; merged implementation still has paths warranting guard review. |
| !8899 | merged | scanned | Include sharkd in warning-clean build target set. |
| !8898 | merged | deep | Balanced Flex/Lemon diagnostic macros at generated-code boundaries; compiler portability. |
| !8897 | merged | deep | XNAP 28/36-bit cell IDs: hf masks plus proto_tree_add_item ensure correct PDML/JSON exports. |
| !8896 | merged | deep | NGAP masked cell IDs and source ASN.1 regeneration correct exported semantic values. |
| !8895 | merged | deep | S1AP masked 28-bit cell identity correct tree/export values. |
| !8894 | merged | deep | HTTP2 URI unescaped header may be invalid UTF8; validate/sanitize after percent decode. |
| !8893 | merged | scanned | Temporary Windows GitHub CI CMake downgrade, another branch variant. |
| !8892 | merged | discussion | QCustomPlot upstream update and reapplied downstream fixes; Alexis leaves squash choice to author. |
| !8891 | closed / unmerged | discussion | TCP retransmission/desegmentation proposal superseded by !17660; Alexis commit author/message guidance. |
| !8890 | merged | deep | Introduce format_char for arbitrary IAX2 byte display with escaped valid UTF8. |
| !8889 | merged | scanned | AT command names use format_text rather than raw byte strings. |
| !8888 | merged | deep | T.30 reverse-ordered number decoding now converts to UTF8 after stripping. |
| !8887 | merged | discussion | cppcheck modulo-one tautology in PTP corrected to modulo two with Lars Völker's domain confirmation. |
| !8886 | merged | scanned | Release notes remove stale claim about display-filter feature withdrawal. |
| !8885 | merged | scanned | Suppress Clang documentation warning in Flex-generated code. |
| !8884 | merged | deep | RFC 5149 UTF8 and 3GPP label-encoded APN conflict; João corrected invalid assumption, John retained documented 0x20 heuristic. |
| !8883 | merged | scanned | Raise CMake baseline to 3.13 across supported platforms and simplify Python detection (historical). |
| !8882 | merged | scanned | Correct third-party QCustomPlot Doxygen HTML tag/warning syntax. |
| !8881 | merged | deep | Locamation text line parsing via tvb_find_line_end and explicit UTF8 encoding. |
| !8880 | merged | discussion | Re-enable GCC Release O3 warnings to evaluate previously muted diagnostics. |
| !8879 | merged | scanned | Fix QCustomPlot Clang documentation-html warning. |
| !8878 | merged | deep | AOL two-byte ASCII token registered as FT_STRING, not numeric FT_UINT16. |
| !8877 | merged | deep | Locamation EOL included in highlighted byte span but excluded from displayed text; John Thacker explained difference and backport. |
| !8876 | merged | deep | Enable Clang and GCC Release CI warnings-as-errors. |
| !8875 | merged | scanned | Add initial Lemon -Wunreachable guard, superseded by more systematic diagnostic bracketing. |
| !8874 | merged | deep | Limit Werror compiler flags to handwritten sources, excluding generated Flex/Lemon. |
| !8873 | merged | scanned | tfshark warning-clean target and unreachable-code repair. |
| !8872 | merged | deep | Peter Wu discussed Clang linker selection via per-target CMake linker flags and -Werror compile probes. |
| !8871 | merged | scanned | ETW extcap tooltip spelling correction. |
| !8870 | merged | scanned | Remove legacy cmake3 RPM package fallback from supported distro setup (backport). |
| !8869 | merged | deep | Locamation line parser and UTF8 handling corrected on master. |
| !8868 | merged | deep | X509IF NULL DN accessor guard and valid initial string buffer follow-up. |
| !8867 | merged | discussion | Mixed warning cleanup; source includes questionable commented-out free calls, not a broad ownership precedent. |
| !8866 | merged | scanned | Remove legacy cmake3 RPM package fallback from setup on master. |
| !8865 | merged | scanned | Disable retiring openSUSE 15.3 CI jobs. |
| !8864 | merged | deep | O-RAN U-plane PRB stride recalculated per section after udCompHdr and modulation compression special case. |
| !8863 | merged | deep | X509IF replace fixed buffers with wmem_strbuf to avoid UTF8 truncation; !8868 fixes initialization/NULL. |
| !8862 | merged | deep | TCP modular LT_SEQ ACK frontier fix; subsequent !8988 discussion shows remaining hole/contiguity issue. |

## Verification and follow-up

Analyzed each stored MR's metadata, human discussion, and changes/diff material, inspecting longer diffs where needed. No compile, runtime test, fuzz, or live GitLab recheck was performed for this historical documentation review. Next expected corpus candidate: **!8861**, present in this pinned corpus snapshot; recheck complete tracking membership before future selection.
