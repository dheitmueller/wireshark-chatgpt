# Supplemental Reviewed Wireshark Merge Requests

This file supplements `reviewed-mrs.md` for recurring corpus-review runs. Consult both ledgers before selecting merge requests so already-reviewed MRs are not repeated. Entries here may later be folded into the main ledger.

## Corpus commit 2ed161d1

Corpus commit: `2ed161d19398e620fa89227724ed928d4f5edf27`

| MR | Status | Notes |
|---|---|---|
| !25788 | Scanned | release-4.6 backport of the ERF time-tag bounds fix already represented by !25786. Merged; validates tag length before fixed-size copy, with no new substantive review evidence. |
| !25789 | Scanned | release-4.4 backport of the same ERF time-tag bounds fix. Merged; no additional lesson beyond !25786. |
| !25790 | Scanned | Wireshark 4.7.2 / Stratoshark 0.10.2 build and release-note preparation. Merged with CI intentionally skipped; release-maintenance plumbing, no substantive human review. |
| !25791 | Scanned | Version preparation for Wireshark 4.7.3 / Stratoshark 0.10.3, including project/library version updates. Merged with CI intentionally skipped; no reusable engineering lesson. |
| !25792 | Scanned | Release-note initialization for 4.7.3 / 0.10.3. Merged; no substantive human review or additional convention. |
| !25793 | Scanned | Megaco encoding typo fix: replaces `ENC_STR_NUM` (a mask) with the concrete `ENC_STR_HEX` value for `tvb_get_string_uint`. Merged; useful API-correctness example but no broader convention extracted. |
| !25794 | Scanned | UET TSS entropy-header offset fix. Merged; author supplied a focused pcap demonstrating both IP/Entropy/TSS and IP/UDP/TSS before/after behavior, reinforcing existing capture-based validation practice. |
| !25796 | Scanned | Geneve GCP/FOPiC timestamp option. Merged; included example pcap and expected text output plus manual verification. Reinforces existing submission/testing guidance without adding a new rule. |
| !25798 | Discussion-focused | BGP MUP SAFI draft update. Alexis La Goutte requested a pcap; contributor supplied one after merge. Independent corroboration of the already high-confidence sample-capture expectation for protocol changes. |
| !25799 | Discussion-focused (closed) | Couchbase GET_EX/GET_EX_REPLICA support was submitted from fork `master`. Alexis instructed the contributor to close it and reopen from another branch. Reinforces the established named-topic-branch workflow rule; closed/unmerged, so not an implementation exemplar. |
| !25801 | Scanned | Windows libgcrypt bundle update to 1.12.2-3 to avoid repeated DLL load/unload behavior. Merged; dependency-package maintenance with no substantive human review. |
| !25802 | Scanned | release-4.6 backport of !25801's libgcrypt package update. Merged; no additional lesson. |
| !25804 | Scanned | Clean resubmission/successor of the Couchbase GET_EX/GET_EX_REPLICA change after the earlier fork-master MR was closed. Merged; confirms the implementation itself was straightforward once branch hygiene was corrected. |
| !25805 | Scanned | Qt QPointer optimization using C++14 init-capture and a Qt-version guard to move when Qt >= 6.6 supports the desired constructor behavior. Merged; no reusable dissector/review convention extracted. |
| !25806 | Deep | rlogin invalid-text fix. Replaces raw fixed-buffer `tvb_memcpy()` string handling with encoding-aware `tvb_get_string_enc(..., ENC_ASCII)` in file-scope memory so invalid characters are sanitized before later column formatting. Added packet-derived string/column safety guidance to `dissector-conventions.md`. |
| !25807 | Deep | LBM SRS heuristic safety fix authored by John Thacker. Explicitly states that heuristic dissectors must not throw exceptions for packets that do not belong to them; replaces unsigned subtraction-based length tests with `tvb_captured_length_remaining()`. Added as a durable heuristic-dissection rule. |
| !25808 | Scanned (closed backport) | First release-4.6 backport attempt of !25807. Closed after failed pipeline; superseded by merged !25809, so not an implementation exemplar. |
| !25809 | Scanned | Successful release-4.6 backport of !25807. Merged and independently reinforces the heuristic/no-exception and overflow-aware remaining-length rule. |
| !25811 | Scanned | release-4.6 backport of !25806 rlogin invalid-text fix. Merged; no additional lesson beyond the master change. |
| !25812 | Scanned | release-4.4 backport of !25806 rlogin invalid-text fix. Merged; no additional lesson beyond the master change. |
| !25813 | Deep | RDP decompression bounds hardening. Rewrites checks into overflow-safe forms such as `count > limit - outputCount` and validates remaining source/output capacity before copies. Merged; strongly corroborates the notebook's existing subtract-before-compare/remaining-capacity arithmetic guidance. |
| !25814 | Discussion-focused | C12.22 bounds fix. Guy Harris asked whether the BER-encoded portion was substantial enough to use Wireshark's ASN-to-dissector generator; the fixer explicitly lacked protocol expertise and kept the MR to the reported out-of-bounds bug. Merged. Useful architectural question from Guy, but no resulting implementation change, so do not yet generalize it into a rule. |
| !25815 | Scanned | 3GPP 32.423 nettrace dispatch refactor replaces hard-coded branching with a declarative mapping table supporting direct dissector names and dissector-table dispatch, with unknown protocols falling through for best-effort handling. Merged; clean extensibility refactor but no substantive human review. |
| !25816 | Discussion-focused | TTL segmented-message bounds hardening. John Thacker noted that GUI-facing wiretap error text should identify the file format/module rather than internal function names, and that `WTAP_ERR_INTERNAL` should represent a condition impossible even for malformed input—otherwise validation belongs earlier and the module still has a bug. The author proposed a separate master-only cleanup rather than expanding this safety fix. Merged; useful wiretap/error-semantics evidence, retained as contextual guidance pending corroboration. |
| !25817 | Scanned | Follow-up 3GPP nettrace refactor splits XML attribute parsing and exported-PDU construction out of a monolithic function, leaving a short orchestration path. Merged; maintainability improvement with no substantive review discussion. |

## Corpus commit a67598e5

Corpus commit: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

| MR | Status | Notes |
|---|---|---|
| !25818 | Scanned | 3GPP nettrace timestamp and memory-leak cleanup. Merged; uses quotient/remainder carry for millisecond overflow and frees XML resources on all open paths. No substantive human review. |
| !25819 | Scanned | Replaces fragile raw-buffer `strstr`/pointer parsing of session XML with bounded libxml2 parsing and proper resource cleanup. Merged; good robustness refactor but no substantive human review. |
| !25820 | Scanned | Deduplicates session-tag lookup shared by sequential and seek reads into one helper. Merged; straightforward maintainability cleanup. |
| !25821 | Scanned | release-4.6 backport of the TTL segmented-message bounds fix represented by !25816. Merged; no additional lesson. |
| !25822 | Scanned | Removes a duplicate `nds_data_handle` declaration that newer GCC rejects. Merged; compile-portability cleanup with no review discussion. |
| !25823 | Scanned | Automatic release-4.6 data/translation update. Merged; no reusable engineering lesson. |
| !25824 | Scanned | Automatic master data/translation update. Merged; no reusable engineering lesson. |
| !25825 | Scanned | Automatic release-4.4 data/translation update. Merged; no reusable engineering lesson. |
| !25826 | Scanned | release-4.6 backport of the RDP bounds hardening already captured from !25813. Merged; no additional lesson. |
| !25827 | Scanned | release-4.4 backport of the RDP bounds hardening already captured from !25813. Merged; no additional lesson. |
| !25828 | Scanned | 3GPP nettrace cleanup: data-driven transport mapping, flatter file-open parsing, and thread-safe one-time regex initialization. Merged; solid refactor but no substantive review discussion. |
| !25829 | Scanned | Reorders display-filter operator documentation so related operator sections are adjacent. Merged; documentation-only. |
| !25830 | Scanned | CI temporarily installs `aqt` from its GitHub repository to gain Qt 6.11 support unavailable in the published PyPI version. Merged; build-infrastructure maintenance. |
| !25831 | Discussion-focused | TTL wiretap error-message cleanup. Follow-up discussion distinguishes true internal invariant failures from bad-file/user-misconfiguration cases; Pascal Quantin proposed keeping ambiguous lookup failures as `WTAP_ERR_BAD_FILE`. This independently corroborates !25816, so the wiretap error-semantics rule was promoted into `architecture.md`. |
| !25832 | Scanned | release-4.6 Asterix fix to fetch values only for field types that use them. Merged; backport with no new review evidence. |
| !25833 | Scanned | Display-filter Flex rule prevents a trailing backslash from falling through to Flex's default console-printing rule. Merged; lexer-specific fix with no broader convention extracted. |
| !25834 | Deep | 3GPP nettrace second-pass fix. Sequential read records session context per packet keyed by stable `data_offset`; `seek_read()` restores that context because its local `<msg>` buffer cannot reconstruct the preceding session. Added as a durable wiretap random-access state rule in `architecture.md`. |
| !25835 | Scanned | Compresses the WCA banner PNG. Merged; resource-only change. |
| !25836 | Scanned | release-4.6 backport of the WCA Qt welcome-page slide. Merged; no reusable engineering lesson. |
| !25837 | Deep | Core reassembly bounds fix by John Thacker. Overlap comparison is clipped to the known allocated `datalen` before `memcmp()` because fragments can extend beyond the assembled buffer. Added as a durable reassembly-buffer invariant in `architecture.md`. |
| !25838 | Discussion-focused (closed) | release-4.4 backport attempt of the Asterix field-fetch fix. Guy Harris explicitly closed it as unnecessary after verifying that the release-4.4 tip did not exhibit the bug. Strong backport-process evidence, but the unmerged patch is not an implementation exemplar. |
| !25839 | Discussion-focused (open) | Startup-profile preference feature remained open with unresolved regression concerns from Stig Bjørlykke, including needless preference-name migration breakage, last-used-profile state synchronization, and UI placement/scope. Useful review evidence but low weight until resolved/merged. |
| !25840 | Scanned | Display-filter UCN semantics aligned with C++11+ behavior for escapes inside string/character literals, with tests and documentation updated. Merged by John Thacker; no substantive human review. |
| !25841 | Deep | Preference application is split into effect discovery and mutation: compute redissection flags first, freeze the packet list when required, then apply stashed values. Merged; prevents dissectors observing new preferences while dependent resources/state are still stale. Promoted to `architecture.md`. |
| !25842 | Scanned | release-4.6 backport of the C12.22 bounds fix already represented by !25814. Merged; no additional lesson. |
| !25843 | Scanned | release-4.4 backport of the same C12.22 bounds fix. Merged; no additional lesson. |
| !25844 | Deep | John Thacker's merged `add_seq()` reassembly hardening uses checked addition for aggregate fragment size, caps to the supported `INT32_MAX` domain, and clips later copies to actual remaining capacity. Promoted to `architecture.md` and `dissector-conventions.md`. |
| !25845 | Scanned | Makes display-filter operators case-insensitive while preserving case-sensitive escape prefixes and updates reserved-name checks/tests. Jaap Keuter's explicit boolean-return style suggestion was applied; too narrow for a broader rule. |
| !25846 | Scanned | release-4.6 backport of !25844's reassembly-length hardening. Merged; corroborates the master rule without adding new discussion. |
| !25847 | Scanned | release-4.4 backport of !25844's reassembly-length hardening. Merged; corroborates the master rule without adding new discussion. |
| !25848 | Scanned | BT ATT Read Multiple Variable Response loop is bounded by the handle count stored from the matching request instead of iterating indefinitely over response data. Merged; reinforces the general requirement to bound response decoding by stored request-context capacity. |
| !25849 | Scanned | release-4.6 backport of !25848. Merged; no additional lesson. |
| !25850 | Scanned | release-4.4 backport of !25848. Merged; no additional lesson. |
| !25851 | Scanned | NVMe-MI command-body enhancement persists request DTYP/CONFIGID context for response decoding and keeps a large feature series split by functional area. Merged; useful architecture example but no substantive review discussion beyond approval. |
| !25852 | Deep | UET Rendez-vous field-layout fix. Anders Broman requested 32-bit registered fields with appropriate bitmasks so the tree corresponds more directly to the specification; author supplied spec evidence, before/after screenshots, and a focused pcap. Added to `dissector-conventions.md`. |
| !25853 | Deep | TLS nesting refactor distinguishes the current TLS session from the parent/calling TLS session and passes an explicitly resolved `SslDecryptSession *` into helpers. Merged; promoted as an API/context-selection rule in `architecture.md`. |
| !25854 | Scanned | Recent capture-file paths are normalized lexically to absolute/clean/native form without requiring the file to exist, avoiding `canonicalPath()` semantics that depend on filesystem existence. Merged; useful Qt state handling but too narrow for a general Wireshark rule. |
| !25855 | Deep | UET PSN presentation separates wire-backed offset fields at their true packet byte positions from calculated PSNs, which are separate generated tree items. Merged with focused screenshots/pcap; promoted to `dissector-conventions.md`. |
| !25856 | Scanned | Adds validated E.212 IMSI tap records plus Qt and tshark consumers. Merged; producer-side IMSI validation prevents bad session-derived strings reaching tap consumers, but no substantive review discussion justified a new general rule. |
| !25857 | Scanned | release-4.6 backport of !25841's preference-state ordering fix. Merged despite backport conflicts; corroborates the master behavior without additional review guidance. |
