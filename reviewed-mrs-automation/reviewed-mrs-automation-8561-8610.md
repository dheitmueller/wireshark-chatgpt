# Automated Wireshark MR review — !8610 through !8561

**Review date:** 2026-10-31. **Model:** GPT-6 (newer than GPT-5.6 Sol).
**Corpus repository:** `dheitmueller/wireshark-corpus-mrs`
**Exact corpus commit:** `ddcaa22b51c68f594e425a23388c3a2086813054`
**Notebook baseline commit:** `d9ddf593e260ddf77bf54842f62d1233b6153fe7`
**Review direction:** descending, the highest-numbered previously unreviewed corpus MRs first.
**Exact reviewed count:** **50 distinct MRs**; **46 merged**, **4 closed/unmerged** (!8586, !8581, !8580, !8570).

## Reviewed-set construction and tracking

Before selection, inventoried all 374 existing files in `reviewed-mrs-automation/`, the ordinary range ledgers and all exceptional/noncontiguous/backfill/gap/exact-membership ledgers, and consulted both `reviewed-mrs.md` and `reviewed-mrs-automation/reviewed-mrs-automation.md`. Checked the preceding exact-membership ledger (!8660 through !8611) and queried for candidate overlaps rather than inferring reviewed membership from an interval name. The old ledger identifies !8610 only as the *next unreviewed candidate*. Corpus tree enumeration confirms that all 50 separate files `mr_8610.json` through `mr_8561.json` exist at `ddcaa22b51c68f594e425a23388c3a2086813054`, and that `mr_8560.json` is also present. None of these fifty is counted as reviewed in the consulted tracking. No numeric range was treated as wholly reviewed based solely on a filename. The 50 corresponding stored MR JSON records, including changes/diffs, descriptions, outcomes, and human discussions, were inspected.

The previously completed **!17571–!17620 batch remains preserved and counted**, with exactly 50 unique historical ledger rows and unchanged blob SHA `9393377c3adeb3a38d502532839765e3abe771fe`. This run does not edit that or any other preexisting review ledger. This is analysis of the pinned historical corpus, not live GitLab state verification, capture replay, compilation, or independent fuzzing.

## Exact 50 MR outcomes and findings

| MR | Corpus outcome | Review depth | Finding |
| --- | --- | --- | --- |
| [!8610](https://gitlab.com/wireshark/wireshark/-/merge_requests/8610) | merged | Scanned | Guy Harris release-4.0 backport of !8609 high-bit ASCII test; portable regardless of char signedness. |
| [!8609](https://gitlab.com/wireshark/wireshark/-/merge_requests/8609) | merged | Deep | Guy Harris master: octet is ASCII iff its 0x80 bit is clear; cleaner than signed-char/control classification. |
| [!8608](https://gitlab.com/wireshark/wireshark/-/merge_requests/8608) | merged | Scanned | Guy Harris release-3.6 backport of !8606 portable character classification. |
| [!8607](https://gitlab.com/wireshark/wireshark/-/merge_requests/8607) | merged | Scanned | Guy Harris release-4.0 backport of !8606. |
| [!8606](https://gitlab.com/wireshark/wireshark/-/merge_requests/8606) | merged | Deep | Guy Harris master: plain char signedness varies by toolchain/architecture; accepted first correction, later simplified by !8609. |
| [!8605](https://gitlab.com/wireshark/wireshark/-/merge_requests/8605) | merged | Scanned | Gerald Combs: RPMBUILD_EXTRA_ARGS / __cmake_builddir links RPM CMake directory to ccache and improves CI. |
| [!8604](https://gitlab.com/wireshark/wireshark/-/merge_requests/8604) | merged | Deep | John Thacker: decode slash-terminated hex pairs then the unpacked GSM 7-bit alphabet for UCP/EMI text. |
| [!8603](https://gitlab.com/wireshark/wireshark/-/merge_requests/8603) | merged | Scanned | Gerald Combs ccache configuration trial; supported-directory/absolute-path assumptions revised in later CI changes. |
| [!8602](https://gitlab.com/wireshark/wireshark/-/merge_requests/8602) | merged | Deep | John Thacker: hex parser end offset advances only after complete byte, failure offset is input start. |
| [!8601](https://gitlab.com/wireshark/wireshark/-/merge_requests/8601) | merged | Scanned | Gerald Combs: CI_COMMIT_SHA takes precedence over stash-created commit and HEAD in tarball metadata. |
| [!8600](https://gitlab.com/wireshark/wireshark/-/merge_requests/8600) | merged | Scanned | release-4.0 backport of !8571 uses setup_frame_number for RTCP/SRTP conversation identity. |
| [!8599](https://gitlab.com/wireshark/wireshark/-/merge_requests/8599) | merged | Deep | Configurable AMQP payload subdissector via UAT pattern/regex rows, protocol handle and caller data; sample and schema provided. |
| [!8598](https://gitlab.com/wireshark/wireshark/-/merge_requests/8598) | merged | Scanned | Gerald Combs removes unsupported openSUSE ccache absolute_paths_in_stderr option. |
| [!8597](https://gitlab.com/wireshark/wireshark/-/merge_requests/8597) | merged | Deep | Merged CRC64 PRIu64 change accidentally displays decimal; post-merge complaint; corrected by later merged !8635 PRIx64. |
| [!8596](https://gitlab.com/wireshark/wireshark/-/merge_requests/8596) | merged | Scanned | Gerald Combs makes RPM ccache base_dir and max-size reflect packaging build path. |
| [!8595](https://gitlab.com/wireshark/wireshark/-/merge_requests/8595) | merged | Scanned | release-3.6 backport of CFDP zero-length source filename cursor fix (!8562). |
| [!8594](https://gitlab.com/wireshark/wireshark/-/merge_requests/8594) | merged | Scanned | release-4.0 backport of CFDP zero-length filename fix; Alexis requests 3.6 counterpart. |
| [!8593](https://gitlab.com/wireshark/wireshark/-/merge_requests/8593) | merged | Deep | João Valverde: UTF-8-safe truncation and escaped UCP string presentation instead of mid-character cuts. |
| [!8592](https://gitlab.com/wireshark/wireshark/-/merge_requests/8592) | merged | Scanned | Gerald Combs: expensive MR jobs depend on fast Commit Check gate. |
| [!8591](https://gitlab.com/wireshark/wireshark/-/merge_requests/8591) | merged | Scanned | João Valverde adds common warning-as-error flags to unit-test targets, corrects printf warning. |
| [!8590](https://gitlab.com/wireshark/wireshark/-/merge_requests/8590) | merged | Scanned | João Valverde adds explicit int casts for GLib assertion macro length parameters in CBOR tests. |
| [!8589](https://gitlab.com/wireshark/wireshark/-/merge_requests/8589) | merged | Scanned | IPv4 fragment offset tree uses unmasked original value and separately formatted scaled offset; reviewer requests %u. |
| [!8588](https://gitlab.com/wireshark/wireshark/-/merge_requests/8588) | merged | Scanned | Anders Broman adds H.224 dissector; Stig Bjørlykke requests consistent H.224 registration name. |
| [!8587](https://gitlab.com/wireshark/wireshark/-/merge_requests/8587) | merged | Deep | João Valverde fixes Win64 varint literals with G_GUINT64_CONSTANT before 63-bit shift. |
| [!8586](https://gitlab.com/wireshark/wireshark/-/merge_requests/8586) | closed (unmerged) | Discussion-focused | Closed alternative Windows 64-bit varint-literal fix, superseded by merged !8587. |
| [!8585](https://gitlab.com/wireshark/wireshark/-/merge_requests/8585) | merged | Scanned | Couchbase RangeScan support; post-merge Alexis Analyzer dead-store warning resolved via later !8613. |
| [!8584](https://gitlab.com/wireshark/wireshark/-/merge_requests/8584) | merged | Scanned | H.248 dotted protocol naming fixed in both ASN.1 template and generated dissector/source registration. |
| [!8583](https://gitlab.com/wireshark/wireshark/-/merge_requests/8583) | merged | Deep | TCPCL clamps guint64 lengths before gint arithmetic, reports Expert Info; subsequent platform/test followups required. |
| [!8582](https://gitlab.com/wireshark/wireshark/-/merge_requests/8582) | merged | Scanned | UAT error includes actual unknown Expert Info field name for diagnosable config failures. |
| [!8581](https://gitlab.com/wireshark/wireshark/-/merge_requests/8581) | closed (unmerged) | Discussion-focused | Closed alternate TCPCL Windows compile fix; superseded by merged !8583. |
| [!8580](https://gitlab.com/wireshark/wireshark/-/merge_requests/8580) | closed (unmerged) | Discussion-focused | Closed alternate narrowing/literal fix; Windows test failures remained, superseded by merged followups. |
| [!8579](https://gitlab.com/wireshark/wireshark/-/merge_requests/8579) | merged | Scanned | SAPRouter mainline registration; Alexis Analyzer cleanup and sample capture supplied. |
| [!8578](https://gitlab.com/wireshark/wireshark/-/merge_requests/8578) | merged | Deep | SAP Message Server; Alexis field widths/duplicate hf/deadstore checks; some repeated hf legitimately model repeats. |
| [!8577](https://gitlab.com/wireshark/wireshark/-/merge_requests/8577) | merged | Deep | SAP Enqueue Server; Alexis requests unknown byte fields, width/call checks, Clang cleanup and sample capture. |
| [!8576](https://gitlab.com/wireshark/wireshark/-/merge_requests/8576) | merged | Scanned | Gerald Combs adds ccache for RPM GitLab jobs, precursor to subsequent configuration corrections. |
| [!8575](https://gitlab.com/wireshark/wireshark/-/merge_requests/8575) | merged | Scanned | release-4.0 endpoint geolocation restoration; Chuck reports Country/City/AS Org sorting still broken after merge. |
| [!8574](https://gitlab.com/wireshark/wireshark/-/merge_requests/8574) | merged | Deep | Counted wmem_strbuf UTF-8 validation accepts embedded NUL but checks later bytes, with unit tests. |
| [!8573](https://gitlab.com/wireshark/wireshark/-/merge_requests/8573) | merged | Scanned | Camel TimeAndTimezone ASN.1 template and generated dissector, BCD/quarter-hour decoding and length guards. |
| [!8572](https://gitlab.com/wireshark/wireshark/-/merge_requests/8572) | merged | Deep | Opaque get_dns_name labels converted by individual LDAP/GSUP/SMB callers; alternative blanket ASCII proposal rejected. |
| [!8571](https://gitlab.com/wireshark/wireshark/-/merge_requests/8571) | merged | Deep | John Thacker: RTCP uses same setup frame as RTP when finding/creating multiplexed conversations. |
| [!8570](https://gitlab.com/wireshark/wireshark/-/merge_requests/8570) | closed (unmerged) | Deep | Closed proposed global DNS ASCII replacement; John notes RFC1035-encoded Microsoft LDAP can contain UTF-8; replaced by !8572. |
| [!8569](https://gitlab.com/wireshark/wireshark/-/merge_requests/8569) | merged | Deep | John Thacker/João: opt-in validated Unicode scalar append, replacement for illegal UCS2/UTF16/UTF32 values. |
| [!8568](https://gitlab.com/wireshark/wireshark/-/merge_requests/8568) | merged | Deep | GSM A RR: correct consumed IE length and verify non-byte-aligned padding before labeling as zeros; Pascal objects to misleading label. |
| [!8567](https://gitlab.com/wireshark/wireshark/-/merge_requests/8567) | merged | Scanned | Metamako new trailer fields/CRC64; follow-up decimal-vs-hex regression noted at !8597, fixed !8635. |
| [!8566](https://gitlab.com/wireshark/wireshark/-/merge_requests/8566) | merged | Scanned | release-4.0 backport of Qt byte-view lineSpacing fix (!8563). |
| [!8565](https://gitlab.com/wireshark/wireshark/-/merge_requests/8565) | merged | Scanned | 802.11 beamforming uses Greek glyphs to shorten display; earlier fixed byte limit later superseded by expandable strbuf. |
| [!8564](https://gitlab.com/wireshark/wireshark/-/merge_requests/8564) | merged | Deep | Qt endpoint geolocation columns restored/aligned; text sorting later reported defective in !8575. |
| [!8563](https://gitlab.com/wireshark/wireshark/-/merge_requests/8563) | merged | Scanned | Gerald Combs uses QFontMetrics::lineSpacing consistently across Qt5/Qt6 and Windows Consolas. |
| [!8562](https://gitlab.com/wireshark/wireshark/-/merge_requests/8562) | merged | Deep | CFDP empty source filename still advances past length byte and parses destination; separate enhancement kept out of bugfix. |
| [!8561](https://gitlab.com/wireshark/wireshark/-/merge_requests/8561) | merged | Scanned | Corrects documented NSIS plugin version variables to actual MAJOR_VERSION/MINOR_VERSION. |

## Durable conventions promoted

1. **Text decoding:** merged !8572 implements caller-specific DNS-label text conversion after John Thacker challenged the closed blanket-ASCII approach !8570; merged !8604 decodes UCP hex-encoded GSM 7-bit bytes in two stages; !8569 separates validated Unicode append from raw append after João Valverde's feedback; !8574 validates entire counted buffers including bytes after embedded NUL.
2. **String/API accounting:** merged !8602 reports only complete consumed hex pairs and sensible failure offsets.
3. **User-configurable dispatch:** merged !8599 uses an AMQP UAT with criteria, compiled regex lifetime, named payload dissector and caller-specific data, with reproducible sample/schema/configuration.
4. **Conversation identity:** merged !8571/!8600 uses consistent RTP/RTCP setup_frame_number for lookup and creation.
5. **Portability/presentation:** merged !8587 uses true 64-bit test literals on Windows; merged !8597 was *not* a fully correct CRC64 display fix, as post-merge feedback and later merged !8635 established hex `PRIx64`.
6. **Robustness and review:** merged !8583 clamps unrepresentable protocol lengths and adds Expert Info (with subsequent !8622 corrections); merged !8568 validates bit-aligned GSM padding following Pascal Quantin's objection to unverified labels; !8577–!8579 show detailed Alexis La Goutte field-checker/Clang review and capture expectations, with a legitimate repeated-field counterexample.
7. **GUI model checks:** merged !8564/!8575 restored endpoint geolocation columns but a post-merge report identifies unresolved text sorting, not a completed sorting implementation.

**Evidence weighting:** merged master changes and direct Guy Harris, John Thacker, João Valverde, Pascal Quantin, Alexis La Goutte, and Gerald Combs review outweigh closed alternatives or incomplete first fixes. Stable cherry-picks corroborate rather than multiply independent lessons. Contradicting post-merge reports and subsequent merged corrections are recorded explicitly; no unmerged fix is presented as accepted.

## Next run

**Next expected candidate: !8560**, which exists in the same pinned corpus. Rebuild exact previously-reviewed membership again before selection. The corpus is not exhausted. Keep the scheduled review automation enabled and unpaused.
