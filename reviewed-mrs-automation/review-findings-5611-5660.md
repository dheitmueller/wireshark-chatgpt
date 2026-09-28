# Wireshark MR review findings: !5611-!5660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in this batch were merged. Stable-branch backports are treated primarily as corroboration when the corresponding master change is present.

| MR | Depth | Finding |
|---|---|---|
| !5660 | Deep / negative precursor | RTMPT AMF loop hardening added an iteration budget and propagated zero-length failure to callers so malformed input could terminate enclosing loops. The merged exhaustion predicate was inverted (`if (--iterations)`); John Thacker later called this out and merged !13933 corrected it to test equality with zero. Retain as strong regression evidence, not as the final implementation exemplar. |
| !5659 | Scanned | MAC-NR adds a hidden direction-independent `mac-nr.lcid` field alongside the existing UL/DL fields, improving bidirectional filtering without removing specialized fields. |
| !5658 | Scanned / backport | Guy Harris release-3.4 backport restoring deterministic Kafka offset/length outputs on malformed compact values; corroborates !5643/!5644. |
| !5657 | Deep / high-authority | Guy Harris release-3.4 Kafka hardening: a failed `tvb_get_varint()` returns length 0, so helpers terminate parsing by returning captured length instead of returning the unchanged cursor and permitting non-progress loops. Valid-but-bad lengths still return an offset reflecting bytes actually consumed. |
| !5656 | Deep / high-authority | Guy Harris release-3.4 Kafka decompression limit replaces an arbitrary 50 MB ceiling with the 2^22 maximum used by Kafka's Java LZ4 implementation. Resource ceilings should be justified by protocol/reference behavior when such a bound exists. |
| !5655 | Deep / high-authority | Guy Harris release-3.4 Kafka cleanup eliminates unchecked `-1` offset returns that callers did not handle and initializes optional offset/length outputs on error paths. Helper return/output contracts must match how callers actually consume them. |
| !5654 | Deep / high-authority | Guy Harris simplifies Kafka string handling so the helper returns the display string the caller actually needs, using `proto_tree_add_item_ret_display_string()` so appended text uses the same escaped representation as the tree. Avoid exposing raw offset/length reconstruction details when only semantic display output is required. |
| !5653 | Scanned / backport | release-3.4 NSIS warning for installing 32-bit Wireshark on 64-bit Windows; packaging-only corroboration of !5649. |
| !5652 | Scanned / backport | release-3.6 NSIS warning for 32-bit-on-64-bit installation; packaging-only corroboration of !5649. |
| !5651 | Scanned / backport | release-3.6 Kafka maybe-uninitialized warning fix. The master form is !5650 and the later Guy Harris refactor !5654 is the stronger design evidence. |
| !5650 | Scanned | Initializes Kafka header key offset/length to silence a real maybe-uninitialized path; soon superseded by !5654's stronger semantic-return API. |
| !5649 | Scanned | Master NSIS warning recommending 64-bit Wireshark on 64-bit Windows. |
| !5648 | Scanned | release-3.4 version bump and release-cycle reset; no new engineering convention. |
| !5647 | Scanned | release-3.6 version bump and release-cycle reset; no new engineering convention. |
| !5646 | Scanned | Wireshark 3.4.11 build/release-note generation; no new reusable convention beyond normal release bookkeeping. |
| !5645 | Scanned | Wireshark 3.6.1 build/release-note generation, including the Kafka security fix in release notes; release bookkeeping. |
| !5644 | Scanned / backport | release-3.6 backport restoring Kafka optional output parameters on bad-varint exits; same output-contract lesson as !5643. |
| !5643 | Deep | Master follow-up restores zeroing of Kafka offset/length output parameters on malformed-varint exits after the parser-progress hardening. Terminating an enclosing parse does not remove the helper's obligation to leave advertised outputs deterministic. |
| !5642 | Deep / high-authority | John Thacker migrates text2pcap's private `-d` debug levels to Wireshark's standard logging framework, maps old levels to DEBUG/NOISY, exposes common logging help, updates docs/tests, and keeps `-q` as packet-summary suppression rather than a private log-level switch. |
| !5641 | Scanned | Adds the OpenVPN tls-crypt-v2 client V3 reset opcode based on upstream protocol documentation; protocol-specific update. |
| !5640 | Deep | Gerald Combs adds public `p_set_proto_data()`: update an existing (scope, protocol, key) entry or add one, documents the proto-data API family, uses set semantics for protocol depth, and updates exported symbols. Packet/file proto-data is the preferred shared-state mechanism instead of globals when its lifetime matches the data. |
| !5639 | Discussion-focused | EAP encrypted-IMSI support. Dario Lombardo requested a real capture (supplied), flagged unrelated edits, and with Alexis La Goutte asked the contributor to remove manual AUTHORS changes because author maintenance was automated. Useful submission evidence; implementation is protocol-specific. |
| !5638 | Scanned | João Valverde adds an always-emitted, never-fatal ECHO logging level for `WS_DEBUG_HERE()`; debug convenience behavior is explicitly separated from fatal-threshold semantics. |
| !5637 | Scanned | Refactors absolute-time string formatting through shared helpers; part of the time-formatting series, no independent new rule. |
| !5636 | Scanned | Improves display-filter absolute-time parse errors and adds focused negative tests; useful diagnostics, but later time-parser work supplies stronger architectural evidence. |
| !5635 | Deep | RFC 7468 Wiretap reader is changed from one whole-file packet to one record per encoded structure, supports arbitrarily long logical lines with checked aggregate length, but keeps format recognition bounded to an initial 2048-byte probe. Actual parsing can be complete without making open-time format detection an unbounded scan. |
| !5634 | Scanned | Restores the special NTP zero-time `NULL` representation after time-format refactoring; compatibility-specific follow-up. |
| !5633 | Discussion-focused | AppVeyor VS2019 migration. Gerald Combs rejected making the redistributable-runtime artifact optional merely to make CI pass: missing it would produce installers that fail immediately on user systems. Fix the build environment/discovery instead of weakening a packaging requirement. |
| !5632 | Discussion-focused | Import-from-Hex-Dump IPv4/IPv6 control becomes a combo box. Stig Bjørlykke steered persisted state toward the semantic value `ipVersion=4/6`, not a UI-specific boolean/index, and caught associated label enabled-state handling. |
| !5631 | Deep | Adds explicit UTC semantics to display-filter absolute-time literals and makes Apply-as-Filter preserve UTC/local display semantics. John Thacker explains that historical local-time rules come from the time-zone database and argues for unambiguous ISO-8601 forms; later MRs refine that syntax further. |
| !5630 | Scanned / high-authority style | Guy Harris normalizes the lone text_import helper that did not use the surrounding four-space indentation. Narrow style consistency only. |
| !5629 | Scanned / backport | release-3.6 backport of the Kafka varint non-progress fix from !5626; corroborates the accepted master behavior. |
| !5628 | Scanned | Adds DVB multilingual MPEG descriptors with bounded remaining-length parsing; protocol-specific feature, no substantive human review. |
| !5627 | Scanned | Drops an already-broken AppVeyor Win32 configuration while retaining x64 coverage; CI maintenance. |
| !5626 | Deep | Master Kafka varint hardening: failed variable-length integer decode is terminal for the current parse because a zero decoded-length otherwise leaves offsets stationary. Reports expert info and returns the end-of-capture cursor rather than guessing a fabricated encoded length. |
| !5625 | Scanned | Consolidates USB ID generation by reading libgphoto2 source directly and removes the intermediate Perl extractor/file; build-data maintenance. |
| !5624 | Deep | New OCP.1/AES70 dissector. Jaap Keuter review catches header-field initialization, encoding/type issues, incorrect C return values, unused annotations, and especially that TCP is a byte stream requiring proper PDU desegmentation rather than packet-boundary assumptions. Alexis La Goutte requests a real pcap, which is supplied; Windows CI catches a cast issue. Strong new-dissector checklist corroboration. |
| !5623 | Scanned | Unifies text-import default IPv4 addresses with text2pcap and chooses RFC1918 defaults; narrow consistency improvement. |
| !5622 | Scanned / backport | release-3.4 USB model-list refresh; generated/data update. |
| !5621 | Scanned / backport | release-3.6 USB model-list refresh; generated/data update. |
| !5620 | Discussion-focused | sFlow field labels/abbreviations are corrected to specification terminology; Uli Heilmeier refines the proposal to the more precise “Sampled header length.” Historical field rename evidence, but newer filter-compatibility guidance should govern whether existing abbreviations are deprecated or renamed today. |
| !5619 | Scanned / backport | release-3.4 switches from GLib's compatibility macro to standard C99 `va_copy`; portability cleanup. |
| !5618 | Scanned / backport | release-3.6 `va_copy` cleanup, including logging code; portability cleanup. |
| !5617 | Scanned | Master libgphoto2 model-list refresh; generated/data update. |
| !5616 | Scanned | Extends absolute-time formatting with a flags API and keeps old call spellings as macros over the new `_ex` interface; part of the broader time-formatting refactor. |
| !5615 | Scanned | Refactors duplicated absolute-time formatting into shared helpers; maintainability improvement supporting later time changes. |
| !5614 | Scanned | release-3.6 security release-note/CVE cleanup; no new engineering convention. |
| !5613 | Scanned | release-3.4 security release-note/CVE cleanup; no new engineering convention. |
| !5612 | Deep / negative test evidence | ISO-8601 parser gains support for timezone offsets with or without colon and hour-only forms, plus tests. Those tests encoded the same reversed offset-sign assumption as the implementation; later merged !5668 (John Thacker) corrected both. This is concrete evidence that tests need expected UTC instants derived independently of the code's transformation. |
| !5611 | Scanned | Adds an explicit narrowing cast for `inet_ntop()` size because POSIX uses `socklen_t` rather than `size_t`; localized portability warning fix. |
