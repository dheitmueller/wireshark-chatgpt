# Review findings: Wireshark !7811-!7860

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is weighted above closed work; Guy Harris and other core-maintainer guidance receives the greatest weight.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !7860 | merged | Deep | John Thacker refactors SMTP request handling into a dedicated function as groundwork for handling multiple PDUs and pipelining. |
| !7859 | merged | Scanned | John Thacker moves SMTP response handling into a dedicated function; preparatory and readability refactor. |
| !7858 | merged | Scanned | Automatic master data, translation, and generated-document update; no durable review lesson. |
| !7857 | merged | Scanned | Automatic release-3.6 data and translation update; no additional lesson. |
| !7856 | merged | Scanned | Automatic release-4.0 data, translation, and generated-document update; no additional lesson. |
| !7855 | merged | Scanned | Automatic release-3.4 data and translation update; no additional lesson. |
| !7854 | merged | Discussion | Display-filter syntax errors gain source-location text in a tooltip; Gerald Combs and João Valverde discuss Qt's lack of a practical public QLineEdit squiggle API. |
| !7853 | merged | Scanned | Spelling cleanup changes two pass to two-pass across tests, tools, and CLI. |
| !7852 | merged | Deep | Capture sync-pipe handling moves to GIOChannel watches, replacing platform-specific polling and UNIX-only GLib paths with event-driven common code. |
| !7851 | merged | Deep / Guy | Guy Harris makes the BLF file-format name self-describing and records missing validation and radio-metadata opportunities in Wiretap. |
| !7850 | merged | Scanned | Adds omitted libwiretap Debian symbol entries for newly exported packet-hash helpers; corroborates ABI manifest synchronization. |
| !7849 | merged | Deep / Guy review | John Thacker fixes open-ended subset-TVBuff reported length after offset resolution; Guy flags signed-offset APIs as hazardous for unsigned packet-derived offsets. |
| !7848 | merged | Scanned | release-4.0 Windows libgcrypt 1.10.1 upgrade and discovery/path adjustments. |
| !7847 | merged | Scanned | Coverity dead-store cleanup in DCT2000 parsing. |
| !7846 | merged | Scanned | RLC graph stores seconds in time_t instead of narrowing to guint32; Coverity-driven type correction. |
| !7845 | closed | Low / superseded | release-4.0 TLS SM4-GCM backport closed because later !8175 duplicated and superseded it. |
| !7844 | closed | Low / superseded | release-4.0 ECC-SM4-GCM backport closed because merged !8174 duplicated and superseded it. |
| !7843 | merged | Scanned | release-4.0 QUIC stateless-reset direction fix; only writes from_server after a matching token is found. |
| !7842 | merged | Deep / Alexis review | BGP-MUP support merged after Alexis La Goutte requested a representative pcap and a manually squashed history to make review and backporting easier. |
| !7841 | merged | Scanned | Master Windows libgcrypt 1.10.1 upgrade; counterpart of !7848. |
| !7840 | merged | Deep | HTTP sends known-conversation binary continuation data to Follow HTTP Stream even when normal header recognition or desegmentation is unavailable. |
| !7839 | merged | Scanned | Master QUIC stateless-reset direction fix; counterpart of !7843. |
| !7838 | merged | Deep | TCP distinguishes the packet closing an out-of-order series from retransmission; validated against captures from the referenced issues. |
| !7837 | merged | Scanned | Broad spelling corrections only. |
| !7836 | merged | Scanned | release-3.4 Protobuf fix anchors derived field-name and type items at the original field start with zero source length. |
| !7835 | merged | Scanned | release-3.6 counterpart of the Protobuf source-offset fix. |
| !7834 | merged | Deep | release-4.0 Protobuf source-offset fix; derived semantic metadata no longer claims an unrelated byte extent. |
| !7833 | merged | Scanned | 802.11 Transition Disable KDE support with a focused encrypted sample capture. |
| !7832 | merged | Deep / Pascal review | TLS propagates HMAC-initialization failure to callers, unwinds allocations, and follows the file's established 0/-1 status convention after Pascal Quantin review. |
| !7831 | merged | Scanned | release-4.0 RTPS security PID additions; protocol-specific backport. |
| !7830 | merged | Deep / Alexis review | 802.11 A-MSDU length and padding fix; Alexis requested a pcap and the contributor supplied focused before-and-after validation material. |
| !7829 | merged | Scanned | release-4.0 QUIC stateless-reset support backport. |
| !7828 | closed | Discussion / negative | Pascal Quantin rejects runtime libgcrypt indirection for a compile-old/run-new configuration Wireshark's supported distribution model does not use. |
| !7827 | merged | Deep | RDPUDP retained address state switches to copy_address_wmem(file_scope), corroborating lifetime-matched address ownership. |
| !7826 | merged | Scanned | TLS_SM4_GCM_SM3 decryption support on master with focused sample validation. |
| !7825 | merged | Scanned | ECC_SM4_GCM_SM3 support on master with focused sample validation. |
| !7824 | merged | Discussion | release-4.0 ECC_SM4_CBC_SM3 backport; Pascal identifies prerequisite !7825 and !7826 plus the branch libgcrypt upgrade as dependency context. |
| !7823 | merged | Scanned | Falco bridge adds a defensive NULL check. |
| !7822 | merged | Scanned | release-4.0 macOS packages move to Qt 6.2.4 with release-note, rpath, and deployment changes. |
| !7821 | merged | Scanned | macOS packaging rpathifies QtNetwork because it may link brotli. |
| !7820 | merged | Scanned | macOS packaging works around brotli dylib entries lacking a full path. |
| !7819 | merged | Deep / Pascal review | GSUP adds missing IEs; Pascal and the author deliberately defer a broader pre-existing one-byte-IE length-validation cleanup to a separate MR. |
| !7818 | closed | Low / draft | Draft Follow Stream menu redesign remained unmerged; John identified protocol-selection concerns and Anders invited a future rebase or reopen. |
| !7817 | closed | Low / draft | Draft Qt menu-enablement optimization closed after CI and compiler issues; not accepted architecture evidence. |
| !7816 | merged | Scanned | TCP Accurate ECN implementation; substantial protocol feature without reusable human-review guidance in the snapshot. |
| !7815 | merged | Scanned | macOS Intel CI deployment target corrected to 10.14. |
| !7814 | merged | Scanned | TCP ACK Rate Request option support. |
| !7813 | merged | Scanned | macOS CI explicitly enables Qt6. |
| !7812 | merged | Deep | TCP experimental options migrate to RFC 6994 ExIDs while aliasing the old persisted preference key to the renamed preference and adding a regression capture. |
| !7811 | merged | Scanned | release-4.0 release notes add the SharkFest link. |
