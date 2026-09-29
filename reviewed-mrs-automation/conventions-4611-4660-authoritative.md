# Authoritative conventions — MRs 4611–4660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- Keep `wireshark.h` a deliberately narrow global baseline: no build configuration, no non-installed/private dependency chain, and no upward component dependency. Only facilities genuinely expected everywhere belong there. Merged master !4645 includes direct Guy Harris review.
- Once a facility is intentionally supplied by the global header, source files do not need redundant direct includes solely for that facility; explicit module/API headers still remain necessary. !4645.
- Count capture events at the semantic commit point. `capture_loop_wrote_one_packet()` owns packet-written/captured/sync-pipe increments rather than duplicating them in read and dequeue paths. !4616 by Guy Harris, with !4618/!4623/!4631 corroboration.
- Generated-artifact build-graph changes must be validated across every generator and packaging path that consumes the artifacts. !4612 failed on macOS/Windows, was repaired in !4619/!4620 and partly reverted in !4625/!4626.
- Display-filter syntax should reject ambiguous unquoted text rather than silently reinterpret it. Regexes for `matches` are quoted strings, invalid protocol byte literals fail, and value-level conveniences such as one-byte `0xNN` belong in the byte parser rather than a relation-specific special case. !4632, !4647, !4649.
- Reuse canonical application PDU framing/parsing across transports where the application protocol is identical. !4638 wraps BitTorrent PDU parsing for uTP rather than duplicating it.
- Commit/MR subjects should name the actual component and describe the real operation/need. João Valverde's review of !4648 required `wsutil: install missing public header wsgcrypt.h`.
- A logging/debug macro that suppresses output does not necessarily preprocess away its argument expressions. Declarations referenced by `ws_debug()` still need to compile in `WS_DISABLE_DEBUG` builds. !4633; later !4701 is stronger corroboration.
- Do not treat !4629's first SocketCAN FDF implementation as the final compatibility rule; later Guy Harris MR !4715 hardens it for old captures whose formerly reserved bytes may contain garbage.
- Stable-only backports !4652 and !4655 are corroborating evidence; primary weight should move to master origins !4283 and !4533 when those are reviewed.
