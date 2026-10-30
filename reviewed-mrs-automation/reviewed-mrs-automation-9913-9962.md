# Wireshark MR review ledger — !9962–!9913

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `2e342d36817467b7c72f9df94c15cee2e3883fad`
Review date: 2026-10-30
**Exactly 50 distinct previously unreviewed MRs**: 49 merged; one closed/unmerged (!9930). Direction: descending MR number.

## Selection and duplicate exclusion

Confirmed the corpus branch head remains `ddcaa22b51c68f594e425a23388c3a2086813054` and the corpus contains all 50 JSON records `mr_9962.json` through `mr_9913.json`. Inventoried all **348** notebook review-tracking paths, including per-run ledger names, the exceptional/noncontiguous tracking, `reviewed-mrs.md`, and the supplementary `reviewed-mrs-automation/reviewed-mrs-automation.md`. Consulted aggregate tracking and relevant earlier exact ledgers; previous run's explicit list covers !10312–!10300 plus !9999–!9963, and ends with !9962 as next candidate. The ordinary !10000–!10299 ledgers and latest !10313–!10362 ledger were also checked. A repository code search found no previous review entries for !9913–!9962. The historical `reviewed-mrs-automation-17571-17620.md` was independently parsed and **all 50 distinct rows spanning !17571–!17620 were preserved and counted**.

Selection uses the explicit reviewed-MR membership evidence, not the fiction that any numeric range is wholly reviewed because a filename covers it. In this unchanged corpus, the next fifty unreviewed entries are contiguous !9962 down to !9913. Each selected JSON record's merge outcome, discussion, and change data was examined, with deeper diff analysis for architectural and correctness work. No higher-numbered backfills appeared, and no previously reviewed MR was included in this batch.

## Exact reviewed MR numbers

!9962, !9961, !9960, !9959, !9958, !9957, !9956, !9955, !9954, !9953, !9952, !9951, !9950, !9949, !9948, !9947, !9946, !9945, !9944, !9943, !9942, !9941, !9940, !9939, !9938, !9937, !9936, !9935, !9934, !9933, !9932, !9931, !9930, !9929, !9928, !9927, !9926, !9925, !9924, !9923, !9922, !9921, !9920, !9919, !9918, !9917, !9916, !9915, !9914, !9913

## Per-MR review notes

| MR | Outcome | Depth | Finding |
| --- | --- | --- | --- |
| !9962 | merged | Scanned | TECMP byte-wide Boolean/reserved fields use ENC_NA instead of a meaningless endianness. |
| !9961 | merged | Scanned | UDS offset/length cleanup preserves visibility of trailing undecoded bytes. |
| !9960 | merged | Scanned | VNC RRE removes erroneous two-byte offset advance; release-3.6 backport of !9958. |
| !9959 | merged | Scanned | Same VNC RRE fix on release-4.0; corroborates master !9958. |
| !9958 | merged | Scanned | John Thacker fixes VNC RRE background-pixel offset against RFC 6143. |
| !9957 | merged | Deep | John Thacker: registered Follow Stream types replace hard-coded enum; plugins participate. Post-merge discussion favors disabling empty Follow menu. |
| !9956 | merged | Scanned | Remove unused follow stream enum and function typedef, facilitating registry-driven API. |
| !9955 | merged | Scanned | O-RAN ext6 configuration affects ext11 beamforming bundles; maintainer notes outstanding static/linkage TODO. |
| !9954 | merged | Scanned | UDS packet-scope wmem string allocation in release-4.0; backport of !9949. |
| !9953 | merged | Deep | Emit fieldFilterChanged at selected-field computation; John Thacker requests both Wireshark/Logray paths and clean rebase/squash. |
| !9952 | merged | Scanned | UDS standards terminology and SubFunction filter identifier spelling. |
| !9951 | merged | Discussion-focused | PFCP BBF TR-459 extensions; Alexis La Goutte caught NAT filter typo; fixed before merge. |
| !9950 | merged | Deep | John Thacker snapshots TLS cipher per KeyExchange packet/layer to avoid wrong redissection after renegotiation. |
| !9949 | merged | Scanned | John Thacker replaces leaking g_strdup_printf fallback with packet-scoped wmem_strdup_printf in UDS. |
| !9948 | merged | Scanned | Correct documentation info paths in make-version.py; master. |
| !9947 | merged | Scanned | Correct documentation info paths in make-version.pl; release-3.6. |
| !9946 | merged | Scanned | Correct make-version.py documentation info paths; release-4.0. |
| !9945 | merged | Scanned | Prepare release-3.6.13 version and release notes. |
| !9944 | merged | Scanned | Prepare release-4.0.5 version and release notes. |
| !9943 | merged | Scanned | Sequence-diagram comments must not render in scientific notation; release-3.6 backport. |
| !9942 | merged | Scanned | Same sequence-diagram numeric-format correction; release-4.0. |
| !9941 | merged | Scanned | Build 3.6.12 release and vulnerability changelog/NEWS. |
| !9940 | merged | Scanned | Build 4.0.4 release and release notes. |
| !9939 | merged | Scanned | Prepare 3.6.12 release notes. |
| !9938 | merged | Scanned | Avoid double-escaping sequence-diagram comments; release-3.6 backport. |
| !9937 | merged | Scanned | Same double-escaping fix; release-4.0 backport. |
| !9936 | merged | Scanned | Repair WLAN Traffic help link after user-guide chapter move; release-3.6. |
| !9935 | merged | Scanned | Same WLAN Traffic help link fix; release-4.0. |
| !9934 | merged | Scanned | John Thacker frees both nested RTP ID data and its unused outer allocation; Coverity leak. |
| !9933 | merged | Scanned | Master WLAN Traffic help anchor follows user-guide chapter move. |
| !9932 | merged | Scanned | John Thacker removes premature html_escape because ElidedLabel already escapes. |
| !9931 | merged | Scanned | John Thacker sets sequence comment axis to fixed numeric formatting for string-like labels. |
| !9930 | closed/unmerged | Discussion-focused | Manual generated SRVSVC repair rejected as precedent; Alexis requires IDL/CNF, separate accepted successor !10009. |
| !9929 | merged | Scanned | User-guide Manage Interfaces anchor adjusted to the help action target. |
| !9928 | merged | Scanned | Qt RTP selected IDs copied by value; replace heap temporary with stack value. |
| !9927 | merged | Scanned | Qt RTP analysis cleanup releases tab name and temporary calculation data. |
| !9926 | merged | Discussion-focused | Prepare 4.0.4 release notes; John Thacker points out prior fixed bug for release notes. |
| !9925 | merged | Deep | John Thacker rejects signed negative bit counts before byte-rounding, preventing -1..-7 bypass. |
| !9924 | merged | Discussion-focused | DRDA SQL statement length exposed; John Thacker asks about EBCDIC-specific lengths, addressed. |
| !9923 | merged | Scanned | John Thacker parents RtpAudioGraph QObject to QCustomPlot owner. |
| !9922 | merged | Scanned | Clang Analyzer dead-store correction in pcapng Darwin option decoder. |
| !9921 | merged | Deep | John Thacker distinguishes borrowed stack-local RTP address lookup keys from owned list entries. |
| !9920 | merged | Scanned | Clarify g_atomic_pointer TSAN false positives for GLib <2.64. |
| !9919 | merged | Deep | sharkd SSRC hex representation matches analyze/download parser; missing RTP returns error; capture and 3 tests; Gilbert suggests error-code docs. |
| !9918 | merged | Deep | sharkd returns RPC status/error on truncated capture load rather than hanging; test with truncated PCAP. |
| !9917 | merged | Scanned | ICMPv6 ND lifetime fields retain numeric value and append human-readable seconds. |
| !9916 | merged | Scanned | RTPS internal helper receives static linkage. |
| !9915 | merged | Scanned | Routine generated PCI/manufacturer/enterprise registry updates; service update failed. |
| !9914 | merged | Deep | John Thacker reverts generic nested-dissector conversation-state restoration; final pinfo context required by GUI. |
| !9913 | merged | Scanned | John Thacker uses ws_debug to remove dfilter tracing overhead from release builds. |

## Durable conventions promoted

- **!9950 (merged, John Thacker):** persist the cipher that was in force *at each handshake* in per-packet/layer data; the conversation's later cipher cannot safely decode earlier packets on redissection. Added to `protocol-context-state-conventions.md`.
- **!9914 (merged, John Thacker):** distinguish transient per-child context from finalized `pinfo` conversation/address fields consumed by UI/API; blindly restoring all context after the final PDU breaks consumers. Added to `protocol-context-state-conventions.md`.
- **!9925 (merged, John Thacker):** reject signed negative bit lengths before `(no_of_bits + 7) >> 3`, including -1..-7, to prevent a zero-octet validation bypass. Added to `arithmetic-safety-conventions.md`.
- **!9921 (merged, John Thacker):** borrowed RTP packet addresses in stack-local lookup keys must not receive owning free operations; deep-copy addresses retained by a collection. Reinforces `address-copy-lifetime-conventions.md`.
- **!9918 and !9919 (merged):** sharkd must reply to every RPC, including truncated capture and missing RTP data; emit SSRC in the canonical hexadecimal string representation consumed by related methods; regression tests with minimal fixtures. Gilbert Ramirez requested documentation of special RPC errors. Added to `api-error-reporting-conventions.md`.
- **!9957 (merged, John Thacker):** Follow Stream capability should be registry/protocol-ID-driven and plugin-extensible, not tied to a fixed UI enum; Chuck Craft's empty-menu usability report and John's favorable response are explicitly post-merge guidance, not proof of implementation. Added `follow-stream-extension-conventions.md`.
- **!9953 (merged, John Thacker review):** selected-field derived signal emitted at data-change point before focus-dependent return; mirrored in both Wireshark and Logray; review branch rebased/squashed. Added to `platform-gui-conventions.md`.
- **!9930 (closed) and !10009 (previously reviewed merged successor, Alexis La Goutte):** source IDL/CNF and generated C/H must be updated together despite PIDL generator issues; closed generated-only proposal is *not* implementation precedent. Reinforces `generated-code-conventions.md`.
- **!9923 (merged, John Thacker):** parent RTP graph helper to its QCustomPlot lifecycle owner. Added to `qt-object-parenting-conventions.md`.
- **!9913 (merged, John Thacker):** prefer release-elidable `ws_debug` for diagnostic-only display-filter tracing. Added to `logging-conventions.md`.

**Evidence weighting:** merged master changes and explicit maintainer rationale carry most weight. Release backports corroborate without multiplying independent design evidence. Closed !9930 is used solely for explicit review/process direction and its accepted successor !10009, not as an accepted generated-source implementation. No contemporary Guy Harris commentary appears in these fifty records; do not misattribute other maintainers' feedback to him.

## Continuation

Next expected previously unreviewed candidate: **!9912**, subject to re-inventorying any new higher-numbered corpus records and the full reviewed set. Corpus remains nonexhausted. Keep the recurring automation enabled and unpaused.
