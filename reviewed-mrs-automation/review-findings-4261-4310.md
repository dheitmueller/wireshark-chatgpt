# Review findings: MRs 4261–4310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master changes are primary evidence. Stable-branch backports are corroborating evidence, and closed/unmerged work is explicitly down-weighted. Direct maintainer review is weighted by authority and whether the guidance was incorporated or independently corroborated.

| MR | Outcome / weight | Author | Finding |
|---|---|---|---|
| 4310 | merged, master | Roman Donchenko | JPEG inline Exif IFD values. Uses add-and-return APIs for type/count and decodes values stored directly in the four-byte offset slot. Alexis La Goutte asked to avoid macro-generated value-string declarations because explicit declarations are easier to maintain and easier for project check scripts to inspect; the author adopted that review. |
| 4309 | closed, low weight | Chris Caldwell | OptoMMP address-range update. Anders Broman repeatedly required the MR title/commit subject to match the actual change; Graham Bloice recommended squashing the contributor branch before review and splitting unrelated heuristic work from range-table changes. Useful submission-process evidence only. |
| 4308 | merged, master | Roland Knall | Qt dissector-table dialog exposes heuristic descriptions and moves search UI. UI improvement; no durable cross-cutting convention extracted. |
| 4307 | merged, master-3.2 | Gerald Combs | Automatic data/update maintenance on an older branch; no new convention. |
| 4306 | merged, release-3.4 | Gerald Combs | Automatic data/update maintenance; no new convention. |
| 4305 | merged, master | Gerald Combs | Automatic translations/manuf/enterprise/AUTHORS update; no new convention. |
| 4304 | closed/superseded | Devan Lai | Draft Lua FileHandler reload approach. Discussion led to the later merged design in MR 4311, already reviewed in the preceding batch. Treat only as design history. |
| 4303 | merged, master | Ivan Nardi | Disables Follow TLS Stream for QUIC where the TLS-stream UI model is not applicable. Narrow UI correctness change. |
| 4302 | merged, master; high authority | Guy Harris | Fixes the built-in-vs-plugin Wiretap file-type boundary in deregistration. Built-in subtype IDs occupy the range below the first plugin subtype; a boundary predicate must reflect that exact partition. Authored by Guy Harris. |
| 4301 | merged, master | Ed | UBDP header/TLV cleanup. Alexis requested whitespace cleanup and squashing; Stig Bjørlykke asked for a descriptive commit message instead of placeholder 'Update' bodies. Corroborates submission hygiene. |
| 4300 | merged, master | João Valverde | Reassembly unit test casts pointers to void * for %p, matching the C printf contract and eliminating format warnings. |
| 4299 | merged, master | João Valverde | Makes test helper functions translation-unit-local with static linkage. Ordinary linkage hygiene. |
| 4298 | merged, master | João Valverde | Corrects documentation of fatal logging levels. Documentation-only. |
| 4297 | closed/superseded | Chuck Craft | Explores stale applied-display-filter state; later closed after the behavior was fixed by much newer work. Not implementation precedent. |
| 4296 | closed/superseded | John Thacker | Early SMPP midstream-alignment design. Superseded by merged MR 4312, reviewed in the preceding batch; do not weight the abandoned implementation over the accepted one. |
| 4295 | merged, master | John Thacker | H.265 oversized Exp-Golomb handling. Packet-controlled overflow is clamped and diagnosed as malformed rather than reaching DISSECTOR_ASSERT; the implementation also avoids undefined 32-bit shifts and uses an early field-type assertion only for the programmer invariant. |
| 4294 | merged, master | Gerald Combs | Begins POD-to-Asciidoctor man-page migration. Cross-platform/package CI exposed missing Asciidoctor packages, driving capability-conditional build/install logic. Useful early evidence that documentation generators must be validated across packaging environments. |
| 4293 | closed, low weight | Chris Caldwell | Combined OptoMMP heuristic/range proposal; superseded by narrower follow-up attempts. No accepted implementation precedent. |
| 4292 | merged, master | John Thacker | H.264 counterpart to MR 4295: handles oversized Exp-Golomb values without undefined shifts or packet-driven assertions, reports malformed input, and preserves parser progress. |
| 4291 | merged, master | Roman Donchenko | Corrects JPEG/Exif Copyright IFD tag to the specification value. Protocol-specific correction. |
| 4290 | closed, design review | Chuck Craft | Proposed putting tcp.reassembled_in on the final frame. Pascal Quantin rejected self-referential navigation semantics; Ronnie Sahlberg confirmed the field was intentionally a link to the reassembly frame and suggested a separate integer reassembly identifier for grouping. Strong negative semantic-design guidance despite the MR being closed. |
| 4289 | merged, master | Roman Donchenko | JPEG variable-name typo cleanup. No broader rule. |
| 4288 | merged, master | João Valverde | Preliminary MSYS2 support across CMake/tests. Build-platform enablement; later MinGW fixes in the same batch provide the more durable lessons. |
| 4287 | merged, master | David Fort | RDP EGFX/channel work. Alexis caught missing prototypes/header use and style issues; Stig Bjørlykke later reported a macOS typedef-redefinition build failure. Reinforces validating public/header refactors on Clang/macOS as well as the primary platform. |
| 4286 | merged, master-3.2; corroborating high authority | Guy Harris | Stable backport of the USBDump err_info ownership fix from MR 4284. Guy-authored release evidence strengthens the caller-owned error-string contract. |
| 4285 | merged, release-3.4; corroborating high authority | Guy Harris | Second Guy-authored stable backport of MR 4284's USBDump err_info ownership fix. |
| 4284 | merged, master | Roland Knall | USBDump error text is duplicated into owned memory rather than returning a string literal through err_info. The caller-side Wiretap error contract expects releasable heap-owned diagnostic storage. |
| 4283 | merged, master | John Thacker | RPC only performs RPC-fragment defragmentation when the complete transport fragment is available. If TCP desegmentation cannot/will not obtain the missing bytes, protocol-level defragmentation is disabled and the condition is diagnosed. |
| 4282 | closed, low weight | Chris Caldwell | OptoMMP development-history MR with no durable accepted lesson. |
| 4281 | merged, master | Gerald Combs | Makes openSUSE CI package install use no-refresh to match other offline RPM tests and reduce external repository-metadata flakiness. |
| 4280 | closed, low weight | Chris Caldwell | Earlier combined OptoMMP changes; review history only. |
| 4279 | merged, master | Martin Mathieson | Spelling cleanup plus dictionary update. No new convention. |
| 4278 | merged, master | Gerald Combs | POD markup cleanup preceding Asciidoctor migration. Documentation maintenance. |
| 4277 | merged, master | Roman Donchenko | Places each JPEG IFD in its own subtree for clearer presentation. Protocol/UI organization change. |
| 4276 | merged, master | João Valverde | Updates stale Minizip project URL after discussion of provenance/usability. Documentation/dependency metadata, not a new technical rule. |
| 4275 | merged, master | João Valverde | Master-origin Minizip compatibility fix: probes the actual zip_fileinfo member exposed by installed headers because distributions can substitute minizip-ng behind the same package identity. Strong capability-detection evidence. |
| 4274 | merged, master | Uli Heilmeier | Fixes an SSH allocation leak. Straight resource cleanup. |
| 4273 | merged, master; high authority | Guy Harris | Static-analysis wrapper excludes capture-wpcap.c because it is a Windows-only translation unit. Checkers must respect the build matrix instead of parsing/compiling source that is invalid outside its target environment. Authored by Guy Harris. |
| 4272 | merged, master | João Valverde | Restores no-libpcap build by including the header defining _U_. Its CI failure exposed the checker-scope bug fixed by Guy in MR 4273, tying checker inputs to actual target compilation. |
| 4271 | merged, master | John Thacker | Refreshes PDML/PSML documentation and historical behavior. Documentation maintenance. |
| 4270 | merged, master | Gerald Combs | POD markup cleanup. No new convention. |
| 4269 | merged, master | Gerald Combs | Drops Asciidoctor.js as a supported documentation generator because it lacked required DocBook/PDF/EPUB backends and Ruby macros. Dependency family/name is insufficient; required feature capability matters. |
| 4268 | merged, master | João Valverde | MinGW fixes distinguish MSVC-specific behavior from generic WIN32 target behavior and align feature checks with MinGW definitions. Target OS and compiler/toolchain are separate dimensions. |
| 4267 | merged, master | João Valverde | Removes obsolete CMake-version guards for MinGW target-link options now guaranteed by the supported CMake baseline. Build logic should follow the actual minimum-tool contract. |
| 4266 | merged, master | Constantine Gavrilov | NVMe async-event completion decode. Pascal Quantin suggests proto_tree_add_bitmask()/ret helpers to avoid manual subtree construction and retrieve decoded values; MR merges without requiring that change, so treat it as optional review guidance, not a hard rule. |
| 4265 | merged, master | João Valverde | Further MinGW fixes: uses explicit strftime components instead of unsupported %F/%T extensions and aligns configure definitions with actual runtime behavior. |
| 4264 | merged, master | João Valverde | Normalizes externally supplied target-platform text to canonical case, validates only supported values, derives architecture from validated state, and prints the resolved configuration. Normalize and reject bad build inputs early. |
| 4263 | merged, master | João Valverde | Uses a configure-time run check for C99 snprintf/vsnprintf truncation semantics and fails with explicit target/compiler diagnostics when the runtime contract is unavailable. Detect semantic behavior, not just symbol presence. |
| 4262 | merged, master | John Thacker | Refactors five SDP fmtp string special cases into a declarative lookup table plus numeric fallback and supplies a sample capture. Once exceptions form a set, prefer data-driven lookup over nested special-case conditionals. |
| 4261 | merged, master | Piotr Winiarczyk | Bluetooth Mesh scheduler display improvements. Protocol-specific presentation change. |

## Maintainer weighting

The highest-authority evidence in this batch includes Guy Harris-authored merged MR 4302 (the built-in/plugin Wiretap file-type boundary) and Guy Harris-authored merged MR 4273 (target-aware static-analysis scope). Guy also authored the maintained-branch backports MRs 4285 and 4286 of the USBDump error-info ownership fix. Those are weighted as strong corroboration of the merged master origin MR 4284. Guy's activity in MR 4309 was a title-change event rather than substantive review commentary, so no engineering position is inferred from it.

Other strong evidence includes John Thacker's merged malformed-input hardening in MRs 4292/4295 and RPC reassembly correction in MR 4283; Pascal Quantin and Ronnie Sahlberg's explicit negative semantic review in closed MR 4290; Alexis La Goutte's accepted maintainability review in MR 4310; and João Valverde's merged capability/build fixes in MRs 4275, 4268, 4264, and 4263.
