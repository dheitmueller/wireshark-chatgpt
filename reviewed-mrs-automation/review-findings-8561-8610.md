# Review findings: !8561–!8610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs, working backward from the prior frontier. Merged changes are weighted above closed or superseded work; authoritative maintainer-authored or maintainer-reviewed guidance is given additional weight.

| MR | Outcome | Depth | Review finding |
|---|---|---|---|
| !8610 | merged | Scanned | Guy Harris release-4.0 backport of the ASCII-octet test; duplicate of master !8609 and corroboration of plain-char signedness portability. |
| !8609 | merged | Deep | Guy Harris master fix: test the 0x80 bit directly for ASCII-octet classification, avoiding assumptions about plain-char signedness. |
| !8608 | merged | Scanned | Guy Harris release-3.6 backport explaining that plain `char` signedness is implementation-dependent, notably on ARM. |
| !8607 | merged | Scanned | Guy Harris release-4.0 backport of the same signed-char portability correction. |
| !8606 | merged | Deep | Guy Harris master signed-char portability correction; comments document why the original comparison was not portable. |
| !8605 | merged | Scanned | Gerald Combs exposes `RPMBUILD_EXTRA_ARGS` so CI can control rpmbuild's CMake build directory and improve ccache reuse. |
| !8604 | merged | Deep | John Thacker fixes UCP/EMI decoding by parsing ASCII hex into bytes first, then applying the unpacked GSM 7-bit alphabet. Promoted to text-encoding conventions. |
| !8603 | merged | Scanned | Intermediate RPM ccache tuning; later refined by !8605. |
| !8602 | merged | Deep | John Thacker fixes parser end-pointer/end-offset semantics for partial hex bytes and failed TVBuff string conversion. Promoted to parser API conventions. |
| !8601 | merged | Scanned | Packaging export logic gives `CI_COMMIT_SHA` precedence over a stash-derived commit so CI tarballs are tied to the intended source commit. |
| !8600 | merged | Deep | Release-4.0 backport of !8571: SRTCP lookup/creation uses `setup_frame_number` so RTP/RTCP registration converges on the same conversation. |
| !8599 | merged | Discussion-focused | AMQP adds UAT-controlled payload-dissector dispatch with persisted match criteria; contributor supplied protobuf/config/capture material. Useful extension-point evidence, no new rule promoted. |
| !8598 | merged | Scanned | Backs out an unsupported ccache option on openSUSE; platform capability must be reflected in CI configuration. |
| !8597 | merged | Negative follow-up evidence | Portable printf macro change used `PRIu64` while formatting a hexadecimal CRC. Post-merge user report showed semantic breakage; later-reviewed !8635 corrected it to `PRIx64`. |
| !8596 | merged | Scanned | RPM CI ccache base-dir experiment; part of the sequence culminating in !8605. |
| !8595 | merged | Scanned | Release-3.6 CFDP empty-source-filename fix; duplicate of master !8562. |
| !8594 | merged | Scanned | Release-4.0 CFDP empty-source-filename fix; review requested the 3.6 backport. |
| !8593 | merged | Deep | João Valverde repairs invalid UTF-8 handling in multiple dissectors; corroborates shared encoding/sanitization conventions. |
| !8592 | merged | Deep | Gerald Combs gates expensive MR builds on Commit Check. Promoted to CI resource-scheduling conventions. |
| !8591 | merged | Discussion-focused | Adds `-Werror` to test binaries so test-only code is held to the same warning discipline as production code. |
| !8590 | merged | Scanned | Test-only narrowing casts to satisfy stricter warnings; limited durable value beyond type-domain discipline. |
| !8589 | merged | Deep | Fixes IPv4 fragment-offset presentation after registered mask semantics were effectively applied twice; corroborates existing field-mask rules. |
| !8588 | merged | Discussion-focused | Basic H.224 dissector; review catches protocol/filter naming consistency. No broader rule beyond existing dissector naming guidance. |
| !8587 | merged | Deep | Win64 test fix uses fixed-width `guint64` constants because `unsigned long` is only 32 bits under MSVC LLP64. Promoted to C portability conventions. |
| !8586 | closed | Superseded | John Thacker's ULL literal fix was not the accepted final form; down-weighted in favor of merged !8587. |
| !8585 | merged | Discussion-focused | Couchbase RangeScan support merged, then Alexis La Goutte spotted a Clang Analyzer dead store and contributor followed with !8613; useful reminder to run analyzer before merge. |
| !8584 | merged | Scanned | Corrects H.248 display/table naming punctuation; protocol-specific naming cleanup. |
| !8583 | merged | Deep | TCPCL clamps protocol lengths too large for Wireshark's `gint` domain and emits expert info; review exposed Win64/test issues later corrected by !8587/!8622. |
| !8582 | merged | Scanned | Expert UAT load errors now include the offending field name; corroborates actionable configuration diagnostics. |
| !8581 | closed | Superseded | Competing TCPCL/Win64 fix; Brian Sipos pointed to the broader merged !8583 solution. Down-weighted. |
| !8580 | closed | Superseded | Alternative TCPCL narrowing/literal fix still failed Windows tests and was superseded by merged follow-ups. |
| !8579 | merged | Discussion-focused | SAPRouter import from an external plugin; contributor supplied a sample capture and fixed analyzer feedback before merge. |
| !8578 | merged | Discussion-focused | SAP Message Server import; Alexis used typed-item/checker and analyzer findings, contributor supplied a capture and fixes. |
| !8577 | merged | Discussion-focused | SAP Enqueue import; review asked to expose unknown/reserved bytes, fix typed-item issues, and provide a representative capture. |
| !8576 | merged | Scanned | Initial RPM-package ccache enablement; later refined by !8596/!8603/!8605. |
| !8575 | merged | Scanned | Release-4.0 geolocation-column backport; post-merge report found text-column sorting still missing, so it is not treated as a complete UI exemplar. |
| !8574 | merged | Deep | João Valverde makes counted wmem UTF-8 validation continue across embedded NUL bytes and adds focused tests. Promoted to text-encoding conventions. |
| !8573 | merged | Scanned | CAMEL TimeAndTimezone decoding; protocol-specific ASN.1/generated-source update with explicit malformed-length handling. |
| !8572 | merged | Deep | Caller-specific fix after !8570: `get_dns_name()` remains a raw structural decoder and callers apply their own text encoding. Promoted to wire-text context conventions. |
| !8571 | merged | Deep | John Thacker master conversation fix anchors SRTCP lookup/create to `setup_frame_number`. Promoted to conversation API conventions. |
| !8570 | closed | High-value negative design evidence | Global ASCII validation in DNS-name expansion was withdrawn after John Thacker noted Microsoft LDAP uses the same syntax with documented UTF-8 text; superseded by merged !8572. |
| !8569 | merged | Deep | John Thacker adds an explicit validated code-point append path for charset decoders after João Valverde objected to changing the generic append primitive. Promoted to text-encoding conventions. |
| !8568 | merged | Deep | GSM A RR fix separates true padding semantics from convenient byte display and reuses padding validation rather than presenting misleading bytes; also fixes the consumed-length return. |
| !8567 | merged | Scanned | Metamako trailer update adding new fields/flags; later format-string follow-up is more instructive than the feature change itself. |
| !8566 | merged | Scanned | Release-4.0 ByteView line-height fix uses `QFontMetrics::lineSpacing()` consistently. |
| !8565 | merged | Scanned | 802.11 beamforming display labels shortened with Greek symbols to fit a presentation budget; no durable rule promoted. |
| !8564 | merged | Scanned | Master geolocation-column restoration; post-merge sorting issue was reported on the backport, so treated narrowly. |
| !8563 | merged | Scanned | Master ByteView line-height correction after Qt6 exposed inconsistent height/leading arithmetic. |
| !8562 | merged | Discussion-focused | CFDP empty filename fix advances over zero-length names correctly; Alexis suggested a separate hf enhancement and the author intentionally kept it out of the bug-fix commit. |
| !8561 | merged | Scanned | README.plugins corrects NSIS version variable names; documentation/build integration cleanup only. |

## Durable notebook updates

The strongest promoted rules from this batch are staged decoding for nested wire/text encodings (!8604); caller-owned character-set interpretation when a structural name parser is reused across protocols (!8570/!8572); explicit validated Unicode-scalar append paths (!8569); embedded-NUL-aware validation for counted UTF-8 buffers (!8574); setup-frame anchoring for negotiated conversation registration (!8571/!8600); fixed-width integer constants on LLP64 Windows (!8587); correct end-offset semantics for partial/failing parsers (!8602); and fail-fast CI dependencies that gate expensive jobs on Commit Check (!8592).

The post-merge problem in !8597 is retained as negative evidence only: portable integer-format macros must preserve both width and radix. Later-reviewed !8635 is the accepted correction.

The historical !17571–!17620 batch remains preserved and counted as 50 reviewed MRs.
