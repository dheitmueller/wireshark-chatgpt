# Review findings 459-508

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs were merged. Master changes are weighted above duplicate stable-branch backports; direct maintainer-authored/reviewed evidence receives the greatest weight.

## Strong findings

**!505 RTPS** — Pascal Quantin required per-flow state to avoid mutable globals because globals survive capture changes and collide across simultaneous flows; persistent state should be conversation-scoped when needed. He also asked to cache subdissector handles during handoff, check handles before calls, expose meaningful IDs/lengths/frame-type fields instead of skipping bytes, use normal column APIs, and reuse already-decoded values. Pascal reiterated linear/single-logical-commit history and Graham Bloice requested a standards-compliant commit message. Multiple representative captures were supplied.

**!468 ETW/extcap** — the proposal was substantially redesigned after review by Guy Harris, Dario Lombardo, Graham Bloice, Roland Knall, Pascal Quantin, and Gerald Combs. Dario and Graham rejected an extension-triggered ETL-to-pcapng conversion inside Wiretap because it did not behave like a regular Wiretap reader; the accepted design moved conversion into a Windows extcap. Review also required coherent platform build guards, sample ETL input, NSIS and WiX packaging updates, logical linear commits for extcap/dissector/integration pieces, and later release-note coverage. Guy additionally noted that a native ETL reader requiring whole-file sorting would need an explicit long-open/progress model.

**!484 / !467 / !463 parser progress** — Guy Harris's !484 validates tag end offsets before deriving lengths and stops when invalid framing makes later tag offsets unknowable. !467 terminates FBZERO parsing when a tag cannot make trustworthy progress and rejects non-advancing aggregate offsets. Gerald Combs's !463 independently breaks an LBMSRS loop if a helper consumes less than one byte. Repeated parsers must advance or terminate.

**!483 field/filter naming** — discussion with Gerald Combs, Alexis La Goutte, Anders Broman, Graham Bloice, Christopher Maynard, and João Valverde distinguishes display labels from display-filter compatibility. Graham opposed arbitrary filter renames; João clarified that TCP/IP "Header Length" is a computed semantic value, not merely the raw IHL/Data Offset bitfield. Filter abbreviations should be treated as compatibility-facing identifiers, while field labels should describe the value Wireshark actually exposes.

**!494** — Guy Harris made every `topic_action_url()` return path produce allocated storage because callers free the result. Ownership is part of the API contract and must be uniform across return paths. !495/!496 are stable backports.

**!486** — Guy Harris made switch control flow total even after `g_assert_not_reached()` so Coverity still sees a defined result. Assertions do not replace initialization/control-flow completeness. !492/!493 are backports.

**!506 STUN** — automatic protocol-version selection is per message because different STUN/TURN flavors can coexist in one capture and nested messages can differ from their outer message. A representative Teams capture covers those cases. Pascal also requested semantic alignment between the generated version field's description and filter identity.

**!478 ICMP** — displayed IPv4/IPv6 address subobjects did not advance the parser cursor, so later subobjects were read from the wrong offset. The fix advances by 4 or 16 bytes; Martin Mathieson checked the default/return path. A focused RFC 5837 capture was supplied and !479-!481 backport the fix.

**!475 / !482 checker cleanup** — Martin Mathieson used `check_typed_item_calls.py --consecutive` to find copy/paste filter-abbreviation errors. In !482 he explicitly left uncertain findings unchanged, reinforcing that checker output requires semantic judgment.

**!508 Qt palette** — Gerald Combs replaced construction-time color assumptions with colors derived from the current palette/background and handles application-palette changes. Theme-derived state must follow runtime palette transitions.

## Lower-information or corroborating MRs

!507 ETSI CAT stable fix; !504/!503 XnAP identity fix; !502 XnAP v16.3.0; !501 E1AP v16.3.0; !500 NGAP v16.3.0; !499 X2AP v16.3.0; !498 S1AP v16.3.0. !497 is Guy Harris TODO/design commentary favoring standard encoded-string/proto-item APIs but is not an implemented refactor. !491 is WiX maintenance. !487-!490 are automatic updates. !485 adds Bluetooth HCI ISO decoding/reassembly without substantive review discussion. !477 updates bug URLs. !476 is a narrow `=-`/`-=` correction. !469/!470/!474 centralize URL handling. !471-!473 are backports of !467. !464-!466 update the Lua wiki URL. !462 removes an obsolete QUIC key-log alias after Peter Wu states the old draft is no longer supported. !460/!461 are release/version mechanics. !459 adds TECMP FlexRay CAS support.

No SMPTE ST 291/VANC packet type was encountered.
