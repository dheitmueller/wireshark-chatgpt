# Review findings: !8461–!8510

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs. Merged changes are treated as stronger evidence than closed/superseded submissions; maintainer-authored and maintainer-reviewed guidance is weighted accordingly.

| MR | Outcome | Depth | Review result |
|---|---|---|---|
| !8510 | merged | Scanned | Guy Harris documentation fix warning Windows developers not to select Qt's generic “desktop development” bundle when it installs the wrong toolchain/runtime for the supported Visual Studio build. |
| !8509 | merged | Deep | Corrects swapped 802.11 beamforming phi/psi bit widths; useful protocol-domain fix, later connected to the signed AvgSNR review in !8467. |
| !8508 | merged | Deep | John Thacker makes PER restricted strings produce valid UTF-8, replacing out-of-domain characters instead of exposing invalid text. Promoted to text-encoding guidance. |
| !8507 | merged | Deep | Stable backport of !8503: decode SMB/DirectPlay NUL-terminated UTF-16LE with the normal encoding-aware API rather than keeping every other byte. |
| !8506 | merged | Deep | John Thacker replaces open-coded UTF-8 tail repair in expert formatting with the dedicated `ws_utf8_truncate()` helper. |
| !8505 | closed | Discussion-focused | Gerald Combs rejects compiling a nonfunctional sdjournal extcap when systemd is absent and asks for the actual problem; contributor later discovers stale CMake cache state and drops the MR. Negative evidence only. |
| !8504 | merged | Deep | NetBIOS scope IDs are decoded as ASCII before appending to a UTF-8 name and bounded concatenation is repaired at a UTF-8 boundary. Alexis also catches a new analyzer warning, followed by !8542. |
| !8503 | merged | Deep | Master SMB/DirectPlay UTF-16LE fix; removes a long-standing manual low-byte-only decoder and uses Wireshark's normal text API. |
| !8502 | merged | Deep | John Thacker adds `ws_utf8_truncate()` with an explicit “valid prefix, possibly partial terminal sequence” contract. Promoted to text-encoding guidance. |
| !8501 | merged | Scanned | Aligns DOS date/time conversion with minizip/minizip-ng's 32-bit value domain and corrects the representable year range. |
| !8500 | merged | Scanned | Gerald Combs cleanup of falcodump credential/config parsing; no distinct durable convention beyond established configuration-parsing rules. |
| !8499 | closed | Scanned | Release-3.6 variant of the sdjournal build proposal; Alexis directs the contributor to fix master first, and the MR is closed in favor of !8505. |
| !8498 | merged | Scanned | Improves SCTP Host Name parameter handling; corroborates the later stronger string/value separation work already captured elsewhere. |
| !8497 | merged | Scanned | Updates GitHub Actions to Node-16-capable action releases and plans stable-branch backports after master validation. |
| !8496 | merged | Scanned | Corrects BLF CAN/CAN-FD SRR/RTR interpretation so CAN-FD SRR is not misreported as a Remote Frame. |
| !8495 | merged | Scanned | Stable backport of declaration-format consistency for an exported Wiretap API. |
| !8494 | merged | Scanned | Stable backport of declaration-format consistency; no new rule. |
| !8493 | merged | Deep | João Valverde continues replacing raw `tvb_get_const_stringz()` use with encoding-aware extraction or explicit bounded-string sizing. |
| !8492 | merged | Scanned | Master declaration-style cleanup by Guy Harris; local style consistency only. |
| !8491 | merged | Scanned | Stable documentation grammar correction by Guy Harris. |
| !8490 | merged | Scanned | Stable documentation grammar correction by Guy Harris. |
| !8489 | merged | Scanned | Master documentation grammar correction; no engineering convention promoted. |
| !8488 | merged | Scanned | Guy Harris whitespace/style consistency in a switch; no broader lesson. |
| !8487 | merged | Deep | John Thacker maps illegal UCS-4 code points and incomplete terminal units to U+FFFD so conversion cannot manufacture invalid UTF-8. |
| !8486 | closed | Discussion-focused | Draft text-sanitization fix. João asks for the protocol-aware `get_ascii_string()` / `get_utf_8_string()` helpers; contributor closes after recognizing the issue had already been fixed elsewhere. |
| !8485 | closed | Discussion-focused | JSON invalid-byte fix. João asks to separate the generic UTF-8 helper bug from the JSON bug and advises letting `wmem_strbuf` grow instead of imposing an arbitrary cap; superseded by !8512. |
| !8484 | merged | Scanned | Adds an MSYS2/MINGW64 GitHub CI build. Useful platform coverage but no distinct new convention beyond existing CI-matrix guidance. |
| !8483 | closed | Discussion-focused | John Thacker explains that DNS label syntax should generally be escaped according to DNS rules rather than passed through generic UTF-8 repair. Contributor notes the main change became obsolete and should have been split. Negative design evidence only. |
| !8482 | merged | Scanned | Fixes the Windows console-open registry key, centralizes the key name, and gives the preference an explicit enum type. |
| !8481 | merged | Scanned | Stable backport of FieldInformation label-length handling for Qt 6 embedded-NUL behavior. |
| !8480 | merged | Scanned | Another stable backport of the FieldInformation label-length fix. |
| !8479 | merged | Deep | First part of João Valverde's replacement of `tvb_get_const_stringz()` with APIs that establish encoding/length semantics. |
| !8478 | merged | Deep | John Thacker validates decoded HTTP Basic-Auth data before exposing it as text and adds the decoded bytes as a TVBuff data source; MR explicitly notes RFC charset ambiguity. |
| !8477 | merged | Scanned | Renames the Chinese transport-security protocol from unofficial GMTLS terminology to TLCP based on the published national standard. |
| !8476 | merged | Deep | Precursor to !8502: detects when bounded expert-info formatting cuts a UTF-8 character and removes the partial terminal sequence. |
| !8475 | merged | Deep | Stable backport of Guy Harris's expert-info semantic fix: expert fields are presence markers, not Boolean-valued fields. |
| !8474 | merged | Deep | Stable backport of the same expert-info semantics. |
| !8473 | merged | Deep | Guy Harris master fix: `FT_NONE` expert infos such as TCP retransmission/keep-alive must be tested for presence rather than extracted as Booleans. Promoted to expert-info guidance. |
| !8472 | merged | Scanned | Installs falcodump documentation only with the Logray/falcodump build product; packaging/install correction. |
| !8471 | merged | Scanned | Makes highly repetitive pcapng block logging “noisy” rather than ordinary debug; useful logging hygiene, already covered by later logging conventions. |
| !8470 | merged | Deep | Gerald Combs fixes Qt 6 FieldInformation conversion by resizing the QByteArray to the actual returned display-label length before converting to QString. |
| !8469 | merged | Scanned | Corrects PROFINET PNIO-CM error-code labels from the specification. |
| !8468 | merged | Scanned | Updates the 802.11 standard reference and document link. |
| !8467 | merged | Deep | Alexis La Goutte fixes beamforming AvgSNR as signed `gint8`/integer field; Richard Sharpe confirms and notices the separate phi/psi issue later fixed in !8509. |
| !8466 | merged | Deep | João Valverde replaces locale-sensitive integer parsing with GLib locale-independent conversion and consolidates signed/unsigned error handling. Promoted to locale/parsing guidance. |
| !8465 | merged | Scanned | Expands Windows Asciidoctor executable-name detection for Scoop and related wrappers. |
| !8464 | merged | Scanned | Documentation removes shell prompt characters so build commands are directly copy-pasteable. |
| !8463 | merged | Deep | João Valverde deprecates raw `tvb_get_const_stringz()`, documents safer alternatives, and keeps deprecation warnings non-fatal under Werror where supported. Promoted to text/build guidance. |
| !8462 | merged | Deep | Alexis reports an uninitialized-value warning; John Thacker explains the longer-term architectural direction: remove dissector-specific Follow Stream GUI cases and move protocol-specific navigation behavior into registration. Strong precursor/corroboration for later !9957/!9259 already captured in the notebook. |
| !8461 | merged | Scanned | Removes an unused GIOP assignment found by Clang Analyzer; corroborates existing static-analysis cleanup guidance. |

## Durable findings promoted

- **Encoding and UTF-8 boundaries:** !8502, !8503/!8507, !8506, !8508, !8487, !8478, !8479, !8463.
- **Expert-info semantics:** !8473 with stable backports !8474/!8475.
- **Build warning policy for deprecations:** !8463.
- **Locale-independent numeric parsing:** !8466.
- **Extension-point architecture corroboration:** !8462 anticipates the later accepted Follow Stream registry design already documented from !9957 and !9259.

Closed !8505, !8499, !8486, !8485, and !8483 were retained as qualified negative/superseded evidence only.
