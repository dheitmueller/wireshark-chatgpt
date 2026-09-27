# Review findings: Wireshark MRs !7211–!7260

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Weights: H = strong durable evidence, M = useful accepted evidence, L = scan/process/context. Merged work is weighted above closed work.

- !7260 [merged] [M] Avoid sort work when the physical packet-row set is empty.
- !7259 [merged] [M] Python WSLua tap-generator port; preserve output behavior and explicit config format.
- !7258 [merged] [M] Separate STUN fields when same-width attributes have different semantic value domains.
- !7257 [merged] [H] HTTP chunk framing should be FT_BYTES, not repeated nested data-dissector layers.
- !7256 [merged] [M] Python init.lua generator port; keep template/build dependencies reproducible.
- !7255 [merged] [M] Historical display-filter ambiguity experiment; later parser architecture supersedes it.
- !7254 [merged] [M] Convert lexical range tokens to semantic range objects at the grammar boundary.
- !7253 [merged] [M] Layer-aware display-filter references; compiler review exposed later-fixed uninitialized state.
- !7252 [merged] [H] Master HTTP chunk-layer fix; dissect content only after dechunk/reassembly.
- !7251 [merged] [L] Boolean literal spelling tightened to reduce collision with protocol identifiers.
- !7250 [merged] [M] Backport: duplicate X.509 filter abbreviation is a registry-identity bug.
- !7249 [merged] [M] PTP tolerates recognizable implementation defects while emitting expert warnings.
- !7248 [merged] [H] Python file/subprocess text I/O must specify UTF-8, not inherit locale.
- !7247 [merged] [M] Language baseline bumps should match required features and supported platform baselines.
- !7246 [merged] [M] Convert ws_in4_addr from network byte order before exposing host integer semantics.
- !7245 [merged] [H] Fix duplicate X.509 abbreviation in ASN.1 template and regenerated output.
- !7244 [merged] [L] Default-enable analysis only with understood, bounded protocol-local cost.
- !7243 [merged] [M] Use distinct registered fields when IPv4/IPv6 TLV identities differ semantically.
- !7242 [merged] [L] Presentation typo only.
- !7241 [merged] [M] Account for all DNS payload bytes and expose trailing residue with expert info.
- !7240 [merged] [M] Generator ports should compare semantic output and document intentional whitespace changes.
- !7239 [merged] [H] Guy Harris: consume Wiretap typed packet-verdict options; do not reparse old byte blobs.
- !7238 [merged] [M] AUTHORS generator port preserves UTF-8/output semantics; review newline differences explicitly.
- !7237 [merged] [L] Documentation generator language migration; build wiring preserved.
- !7236 [closed] [L] Unmerged Lua API proposal; no maintainer endorsement, not precedent.
- !7235 [merged] [H] AT_NUMERIC is host-byte-order semantic data; dissectors convert wire order at the boundary.
- !7234 [merged] [L] Replace protocol magic value with named openSAFETY broadcast constant.
- !7233 [merged] [L] Protocol-specific Cisco MCP strict-mode extension; limited durable guidance.
- !7232 [merged] [L] Documentation escaping/typo correction.
- !7231 [closed] [L] Guy Harris: unexplained translation-sentinel edits risk breaking Transifex; reject without rationale.
- !7230 [merged] [L] Compiler-warning cleanup for signed/unsigned Qt comparison.
- !7229 [merged] [M] Conversation UI capability should derive from the actual row/protocol, not a global assumption.
- !7228 [merged] [H] AT_NUMERIC review caught endian ambiguity, PRIu64 portability, and 32-bit Qt conversion overflow.
- !7227 [merged] [L] WSLua Listener debug representation adds tapinfo state.
- !7226 [merged] [L] Field-type docs should be regenerated/checked against the actual ftype inventory.
- !7225 [merged] [M] Use stream-level RTP analysis helper when drift metrics depend on stream counters/state.
- !7224 [merged] [H] Gerald Combs: prefer typed Qt signal/slot connections; compiler catches signature mistakes.
- !7223 [merged] [L] Qt painting off-by-one fix.
- !7222 [merged] [M] Public library API changes require synchronized Debian symbols metadata.
- !7221 [merged] [M] Unicode escape syntax belongs in one documented scanner path.
- !7220 [merged] [M] Master RTP analysis fix; lower-level packet helper omitted required stream state updates.
- !7219 [merged] [M] Expose TCP/UDP stream IDs as semantic model/filter values, not presentation-only text.
- !7218 [merged] [M] Scope Qt-version workarounds tightly and document observed version behavior.
- !7217 [merged] [L] Documentation filename correction.
- !7216 [merged] [L] Terminology/style correction.
- !7215 [merged] [M] Traffic filtering should use unformatted semantic model values.
- !7214 [merged] [M] ABI packaging symbol lists must track public fvalue API changes.
- !7213 [merged] [H] Central type-family predicates should encode taxonomy; avoid scattered one-off exceptions.
- !7212 [merged] [H] Embedded-NUL strings require length-bearing storage, escaping, comparison, and regex APIs end-to-end.
- !7211 [merged] [M] Format the offending value into user-visible errors; do not return a raw format template.
