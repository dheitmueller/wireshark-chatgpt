# Review findings: !7611-!7660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are weighted more heavily than open/closed snapshots. Direct maintainer feedback is given additional authority, especially Guy Harris, John Thacker, Pascal Quantin, Gerald Combs, and other established reviewers.

| MR | Outcome | Depth | Review finding |
|---|---|---|---|
| !7660 | merged | Deep | UMTS RLC UEA0/no-op ciphering. Uses -1 as the unknown sentinel because algorithm value 0 is valid; widens the stored algorithm fields to signed types and checks UEA0 before treating RLC data as ciphered. Also removes an assertion on reset data structures that can legitimately be absent. |
| !7659 | closed | Discussion-focused | Large VS2022/plugin submission. Guy Harris identified many generated/configuration/build outputs that must not be committed, and advised that a dissector intended for upstream shipping should generally be built in rather than added as an external-style plugin. Pascal Quantin noted upstream already built with MSVC 2022 and asked for a clean, relevant patch explaining the real environment difference. Process guidance is authoritative; implementation is not. |
| !7658 | merged | Deep | John Thacker converts SCTP/TCP/UDP port preferences to automatic/range preferences. Existing preference names are preserved: matching table names migrate naturally, while old differently named keys are mapped through deprecated_port_prefs rather than being silently dropped. |
| !7657 | merged | Scanned | Preference cleanup: dissectors whose client/server logic consults configured port preferences now retrieve ranges rather than scalar values, matching the new automatic range-preference representation. |
| !7656 | merged | Scanned | ESP NULL padding validation fix; loop exits correctly on padding mismatch. |
| !7655 | merged | Scanned | Developer Guide extcap documentation cleanup. Reviewer suggestions are editorial rather than new engineering policy. |
| !7654 | merged | Scanned | AT CMGR binary-mode support and SMS PDU parsing. Review caught duplicated field metadata; no broader convention beyond normal field-definition review. |
| !7653 | merged | Scanned | Qt stock reset icon addition for extcap options; no reusable review lesson. |
| !7652 | merged | Scanned | Quake2/QuakeWorld port handling updated to ranges for request/response direction decisions. |
| !7651 | merged | Scanned | MySQL zlib/deflate compressed-packet support. Payload decompression is added without yet attempting downstream payload dissection. |
| !7650 | merged | Scanned | Qt signal/slot correction: use currentTextChanged rather than currentIndexChanged when handler semantics require text; fixes interface toolbar crash. |
| !7649 | merged | Scanned | release-3.6 backport of ESP NULL ICV exclusion fix. |
| !7648 | merged | Deep | John Thacker strengthens ESP NULL autodetection using several independent checks: accepted ICV lengths, padding-length validity, padding-byte validation, and rejection if the candidate subdissector rejects the payload. Good evidence that heuristics should accumulate protocol-consistency tests rather than trust a single weak discriminator. |
| !7647 | merged | Scanned | Master fix excluding non-NULL authentication ICV bytes from the decrypted/cleartext ESP NULL payload. |
| !7646 | merged | Scanned | Windows portability cleanup: prefer compiler-defined _WIN32 over locally defining/testing WIN32. |
| !7645 | merged | Scanned | Protobuf Timestamp presentation corrected when fields are not being exposed as hf entries; keeps useful semantic display available in both modes. |
| !7644 | merged | Scanned | DOCSIS low-latency TLV support; reviewer corrected field presentation wording only. |
| !7643 | merged | Scanned | Rsync automatic port preference cleanup: retrieve configured range and remove stale duplicate preference state. |
| !7642 | merged | Scanned | Removes an obsolete internal Decode As scalar-preference helper after Decode As automatic preferences have all moved to ranges. |
| !7641 | merged | Deep | MySQL prepared-statement unsigned flag fix. Wire format uses 0x80, not boolean 1; contributor supplied a focused capture and before/after tshark output that demonstrates the corrected UINT8 decode. |
| !7640 | merged | Scanned | release-3.6 backport of Qt ByteView hover-preference update and documentation typo fix. |
| !7639 | merged | Deep | BLF ObjectHeader formats 2 and 3. Guy Harris asked whether real files using those formats existed; contributor supplied focused BLF samples for both. Strong corroboration that wiretap format support should be backed by representative real/sample files, especially when adding alternate container/header forms. |
| !7638 | opened | Discussion-focused | Display-filter result cache proposal. Roland Knall listed preference/name-resolution/profile changes that can invalidate cached results; Guy Harris reduced the architectural rule to: anything that forces redissection must flush the cache. Gerald Combs liked the concept later. Open snapshot only, so treat cache design as provisional rather than accepted precedent. |
| !7637 | merged | Deep | John Thacker changes automatic port preferences to ranges even when the default has a single port, aligning preference cardinality with Decode As, which can bind multiple keys. |
| !7636 | merged | Scanned | Automatic preference descriptions now include their default range. |
| !7635 | merged | Scanned | Name-resolution preference names made more consistent and clearer to users; no substantive review discussion. |
| !7634 | merged | Scanned | release-3.4 CMake preset support/backport. |
| !7633 | merged | Scanned | release-3.6 CMake preset support/backport. |
| !7632 | merged | Scanned | release-3.4 backport of IPX name-resolution hash-table crash fix. |
| !7631 | merged | Scanned | release-3.6 backport of IPX name-resolution hash-table crash fix. |
| !7630 | merged | Scanned | Asterix dissector regenerated/updated for specification changes; no substantive review discussion. |
| !7629 | merged | Scanned | Asterix specification converter fix for nested Group items inside Extended items. |
| !7628 | merged | Scanned | DCCP endpoint/conversation table gains independent service/port name resolution. |
| !7627 | merged | Scanned | Master fix for IPX name-resolution hash-table clearing crash. |
| !7626 | merged | Deep | HTTP/2 fake-header override support. Coverity reported apparent leaks for values allocated from pinfo->pool; Pascal Quantin explained that packet-scope wmem ownership frees them automatically and the warning is a false positive. Review static-analysis reports in light of Wireshark allocator lifetime semantics rather than mechanically adding frees. |
| !7625 | opened | Scanned | IO Graph DIRECT(field) proposal for plotting every field instance without aggregation. Open snapshot with no substantive maintainer design conclusion; do not treat as accepted architecture. |
| !7624 | merged | Deep | John Thacker moves HTTP/2 Follow Stream header tap delivery to the point after HPACK decompression. Gerald Combs supplied test expectation updates. Follow output now represents semantically decoded headers rather than compressed framing bytes, and tests explicitly reject the old raw representation. |
| !7623 | closed | Discussion-focused | Superseded Asterix converter MR. Gerald Combs required 'Allow commits from members who can merge'; contributor could not enable it while using protected master, created a topic branch, and resubmitted as merged !7629. Strong workflow evidence for topic branches that permit maintainer fixes/rebases. |
| !7622 | merged | Scanned | release-3.4 spelling-only Qt backport from Guy Harris. |
| !7621 | merged | Deep | MySQL state-machine fix around TLS/AuthSwitch. The pre-TLS LOGIN is effectively STARTTLS-like; after TLS a second LOGIN must remain classifiable as LOGIN rather than inheriting RESPONSE_OK state. Reinforces that protocol state transitions across transport/security mode changes must model semantic phases explicitly. |
| !7620 | merged | Scanned | release-3.4 backport of Guy Harris's capture-comment length validation. |
| !7619 | merged | Scanned | release-3.6 spelling-only Qt backport from Guy Harris. |
| !7618 | merged | Scanned | ZVT receipt-info TLV dissection and correct subtree usage. |
| !7617 | merged | Scanned | Master spelling-only Qt change from Guy Harris. |
| !7616 | merged | Scanned | release-3.6 backport of Guy Harris's capture-comment length validation. |
| !7615 | merged | Scanned | release-3.6 backport of John Thacker's GTP QoS release/version and RADIUS length fix. |
| !7614 | merged | Deep | Guy Harris enforces pcapng's 65535-byte comment-option limit in Wireshark and editcap before accepting user input, while explicitly noting that libwiretap should also defend the invariant because not every caller passes through those frontends. Also notes the limit is format-specific, so a generic global cap would need care for formats that permit larger comments. |
| !7613 | merged | Scanned | DPoE OAM leaf/branch support; review encouraged value-string reuse for enumerated presentation. |
| !7612 | merged | Deep | MySQL AuthSwitchResponse fix: the AuthSwitchRequest-established state was being overwritten before the response was decoded. Supplied capture demonstrates the state-machine failure and correction. |
| !7611 | merged | Deep | John Thacker changes Decode As automatic preference storage from one uint to a range. Decode As can hold multiple port bindings, so scalar storage kept only the last value and could not faithfully clear/restore all mappings; empty range now naturally clears the binding set. |

Count: **50 rows / 50 unique MR numbers**.
