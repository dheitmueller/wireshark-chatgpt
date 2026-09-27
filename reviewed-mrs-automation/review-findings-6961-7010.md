# Review findings for !6961–!7010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work was weighted more heavily than closed work. Maintainer feedback was weighted according to authority and subsystem relevance.

## High-value findings

- **!6981 (merged):** Jaap Keuter explicitly states that `DISSECTOR_ASSERT` is not a protocol error-reporting method. Packet-controlled invalid input belongs on normal malformed-input/error paths; assertions are for implementation invariants.
- **!6979 and !7001 (merged):** Gerald Combs introduces typed arbitrary conversation-element identities and then migrates by-ID conversations to the same mechanism. Retained key data is copied into file-lifetime storage. Conversation identity should match protocol semantics rather than being forced into an address/port tuple.
- **!6993 (merged):** João Valverde stores the logical length of an FT_PROTOCOL value with the value itself and keeps it synchronized when item length is finalized later. Logical extent must not be inferred from a larger backing TVB.
- **!6984 (merged):** John Thacker, João Valverde, and Guy Harris converge on a narrow, documented, version-scoped project diagnostic suppression for a proven GCC/Qt false positive instead of globally weakening warnings or distorting correct code.
- **!6994 (merged):** John Thacker catches that a Qt resource-compiler option is unavailable on older supported Qt. External tool switches must be gated by the first dependency version that actually supports them.
- **!7003 (merged):** John Thacker and Guy Harris give strong rationale for matching nonnegative count/size domains to downstream size/allocation use, avoiding accidental negative signed values becoming very large unsigned sizes.
- **!6969 (merged):** Alexis La Goutte directs wire-backed fields to the ordinary item-decoding API and distinguishes them from values calculated separately.
- **!6968 (merged):** Gerald Combs documents packet-, file-, and epan-scope lifetime contracts directly in the core allocator header.
- **!6995 (merged):** vendored PCRE2/Lua code still receives project source-safety scrutiny, while allocator-family pairing is preserved instead of mechanically replacing a matching deallocator.
- **!6986 (merged):** static mask checking is skipped when the checker could not parse the expression; inability to analyze is not proof of invalidity.

## Per-MR disposition

- !7010 merged — Windows setup cleanup.
- !7009 merged — Windows zstd update.
- !7008 merged — ORAN section type correction.
- !7007 merged — Coverity pointer/member comparison correction.
- !7006 merged — SIP be-route support.
- !7005 merged — PROFINET review distinguished a dissector typo from a protocol change.
- !7004 merged — SIP short-input handling.
- !7003 merged — count/size signedness correction with Guy Harris and John Thacker discussion.
- !7002 merged — direction-specific PDCP expert information.
- !7001 merged — by-ID conversations migrated to element identity.
- !7000 merged — Roon dissector; sample capture and table-driven review improvement.
- !6999 merged — conversation lookup deduplication.
- !6998 merged — local CMake customization documentation.
- !6997 closed — proposed warning-policy relaxation; not accepted precedent.
- !6996 merged — TECMP terminology/filter migration.
- !6995 merged — Lua regex/PCRE2 migration and vendored-code review.
- !6994 merged — Qt resource compiler argument compatibility.
- !6993 merged — protocol-field semantic-length fix.
- !6992 merged — USB speed-aware validation.
- !6991 merged — typed-item checker-driven field corrections.
- !6990 merged — release-note cleanup.
- !6989 merged — automatic generated-data refresh.
- !6988 merged — automatic generated-data refresh.
- !6987 merged — automatic generated-data refresh.
- !6986 merged — checker false-positive prevention for unparsed masks.
- !6985 merged — Guy Harris requested a discoverable protocol specification reference.
- !6984 merged — targeted compiler false-positive handling.
- !6983 merged — Qt6 configuration convenience.
- !6982 merged — Pascal Quantin caught the wrong NACK-range variable in diagnostic text.
- !6981 merged — assertion-versus-protocol-error rule; CBOR/BPv7 tests expanded.
- !6980 merged — LTP statistics feature; requested motivation/sample and later Coverity fix.
- !6979 merged — arbitrary typed conversation elements.
- !6978 merged — BP/TCPCL final RFC references.
- !6977 merged — Falco API update.
- !6976 merged — Falco address-field registration fix.
- !6975 closed — ZigBee contribution; style/unused-field review only.
- !6974 merged — Qt traffic-table usability.
- !6973 merged — traffic type-selection simplification.
- !6972 merged — generated-file spelling-check handling.
- !6971 merged — Qt type selector placement.
- !6970 merged — deterministic simple-statistics default sorting.
- !6969 merged — PROFINET field-decoding API review.
- !6968 merged — wmem scope documentation.
- !6967 closed — conversation flag-domain draft; later merged work is authoritative.
- !6966 merged — BPv7/BPSec correctness work and issue-link review.
- !6965 merged — Qt translation support.
- !6964 merged — Qt-specific traffic UI moved into the Qt frontend.
- !6963 closed — earlier ZigBee submission; no accepted implementation precedent.
- !6962 merged — bounded F5 deployed-version compatibility.
- !6961 merged — corresponding merged F5 compatibility change.

Closed/unmerged MRs in this batch: **!6997, !6975, !6967, !6963**. They were down-weighted relative to merged work.
