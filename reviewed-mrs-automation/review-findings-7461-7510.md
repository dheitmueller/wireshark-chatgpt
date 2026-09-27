# Review findings: !7461-!7510

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master work is primary evidence; release backports are corroborative; closed work is down-weighted.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !7510 | merged | Scanned | WSLua manual include follows the utility-file rename. |
| !7509 | merged | Scanned | Remove obsolete Perl build requirement. |
| !7508 | merged | Scanned | release-3.6 Pod-package cleanup backport. |
| !7507 | merged | Scanned | Master Pod-package cleanup. |
| !7506 | merged | Scanned | Automated master data/translation update. |
| !7505 | merged | Scanned | Automated release-3.6 update. |
| !7504 | merged | Scanned | Automated release-3.4 update. |
| !7503 | merged | Deep | Martin Mathieson required expert info for invalid O-RAN eAxC bit partitions; accepted code verifies four widths sum to 16 and uses bit-offset extraction. |
| !7502 | merged | Scanned | CI moves to Rocky Linux 9 for a supported c-ares baseline. |
| !7501 | merged | Scanned | Generated WSLua docs record source-file provenance. |
| !7500 | closed/superseded | Discussion | HTTP/3/QPACK draft explicitly continued in !9330; not accepted implementation precedent. |
| !7499 | merged | Deep | GLib polling may block in a worker, but GLib dispatch remains on the Qt main thread; bridge is installed only when Qt is not already running GLib. |
| !7498 | merged | Scanned | EtherCAT SDO value-string release-3.4 backport. |
| !7497 | merged | Scanned | EtherCAT SDO value-string release-3.6 backport. |
| !7496 | merged | Scanned | EtherCAT SDO value-string master change. |
| !7495 | merged | Discussion | Exact float filter representation later drew a Stig Bjørlykke revert recommendation; not promoted alone. |
| !7494 | merged | Deep | John Thacker favors shared tvbuff Snappy helpers over per-dissector raw-pointer code; review also calls for a decompressed-output allocation limit and catches Windows include paths. |
| !7493 | merged | Scanned | release-3.6 backport of QUIC initial-connection fix. |
| !7492 | merged | Scanned | Perl becomes optional across normal build/setup paths. |
| !7491 | merged | Discussion | Generated WSLua files become an explicit CMake target and lrexlib dependency, fixing build ordering. |
| !7490 | merged | Scanned | UDS names updated for ISO amendment. |
| !7489 | merged | Discussion | Float display API migration removes BASE_FLOAT, migrates callers, docs, and release notes together. |
| !7488 | merged | Deep | John Thacker fixes bootstrap QUIC connection lookup for multiple Client Initial packets before server response. |
| !7487 | merged | Discussion | WSLua docs generator port validated by comparing generated output. |
| !7486 | merged | Scanned | Fix WSLua argument macro identities. |
| !7485 | merged | Scanned | WSLua generated-markup whitespace cleanup. |
| !7484 | merged | Scanned | release-3.6 Lua/display-filter signal fix. |
| !7483 | merged | Discussion | Guy Harris/Roland Knall question verbose output as a substitute for correct CLI semantics; John Thacker prefers reporting actual written count. |
| !7482 | merged | Discussion | Release notes and build minimums move together, including c-ares 1.14. |
| !7481 | merged | Discussion | Correctly propagate configured DNS TCP/UDP ports to c-ares. |
| !7480 | merged | Discussion | Debian nocheck semantics fixed; DEB_BUILD_OPTIONS is space-separated. |
| !7479 | merged | Deep | John Thacker's QUIC CRYPTO reassembly is tested with real captures for order, overlap, retry, duplicates, missing originals, and both normal and two-pass dissection; discussion preserves QUIC/TLS layered reassembly responsibilities. |
| !7478 | merged | Discussion | Alexis La Goutte asks for squash + focused MySQL pcap; contributor supplies both. |
| !7477 | merged | Scanned | SMPP explicitly displays NULL address range. |
| !7476 | merged | Scanned | Source markup, not generator magic, owns capitalization. |
| !7475 | merged | Discussion | Rich-text license view exposed a Windows follow-up bug; not promoted as design precedent. |
| !7474 | merged | Scanned | RPM setup diagnostics distinguish required/optional packages. |
| !7473 | merged | Discussion | João Valverde says no manual rebase is needed absent conflicts; Alexis requests squash. |
| !7472 | merged | Deep | Accepted portability fix removes struct-packing dependence and uses the protocol's explicit 34-byte sample block size. |
| !7471 | merged | Deep | Implements Jaap Keuter's review: numeric DNS ports use numeric UAT storage and UAT_DEC_CB_DEF instead of strings. |
| !7470 | merged/superseded | Discussion | Compiler-specific packed-struct workaround; Stig Bjørlykke says Wireshark should not depend on struct pack sizes. !7472 is stronger. |
| !7469 | closed/superseded | Discussion | Removing packing outright broke the dissector; negative evidence only. |
| !7468 | merged | Deep | Jaap Keuter pushes DNS ports toward numeric UATs; review also catches that the new c-ares API exceeds the declared minimum version. |
| !7467 | closed/superseded | Discussion | Initial IM2R0 submission closed pending prerequisite fixes; superseded by !7473. |
| !7466 | merged | Deep | Computed Signal-PDU name is exposed as a generated hidden field for filtering/columns without tree clutter. |
| !7465 | merged | Discussion | TLS reassembled-in metadata appears on the first fragment on redissection only when completion is in another frame. |
| !7464 | merged | Discussion | John Thacker replaces local padding macros with shared WS_ROUNDUP_4. |
| !7463 | merged | Scanned | TECMP FlexRay null-frame CRC/payload correction. |
| !7462 | merged | Discussion | TECMP adds a keyed subdissector table with typed context and preserves text/data fallback. |
| !7461 | merged | Scanned | Intermediate lrexlib include-path fix superseded by explicit dependency ordering in !7491. |

Highest-confidence reusable evidence: !7472, !7479, !7494, !7499, !7503, !7471/!7468, and !7466.
