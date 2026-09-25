# Wireshark MR review findings: !9212-!9261

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in this run are merged. Merged implementations therefore carry full positive weight, while later corrective MRs and post-merge comments qualify individual lessons where applicable.

## Durable findings promoted to the notebook

- **!9259 — Follow Stream registration capabilities.** John Thacker moves sub-stream navigation out of Qt/CLI protocol-name special cases and into the registered Follow Stream capability. This exposes a concrete danger of generic code inheriting HTTP/2-only assumptions about sub-stream 0.
- **!9253 — vendored Lemon versus generated parser diagnostics.** The accepted maintenance policy keeps the third-party Lemon executable close to upstream and scopes its exceptional diagnostic handling to that tool boundary, while generated Wireshark parser source remains subject to project checks.
- **!9249, !9254, !9256 — indexed conversation state transitions.** John Thacker fixes removal from the wrong partial-key conversation table before mutating addr2; the latent bug could produce a NULL lookup and segfault. The master fix plus two stable backports make the remove-old-key / mutate / reinsert-new-key ordering strong evidence.
- **!9248 + !9214, qualified by !9224 — malformed text progress accounting.** The ASCII batching optimization in !9224 exposed an adjacent-invalid-byte state bug fixed by John Thacker in !9248. Gerald Combs's UTF-8 fix !9214 was corrected in review by John so remaining length tracks all consumed source bytes, not only valid output.
- **!9243 — checker API-family coverage.** Martin Mathieson enables typed-item checks for the bit-addressed protocol-tree APIs so an API spelling cannot silently escape the same field contract.
- **!9238 — compiler identity and unaligned access.** John Thacker uses compiler-specific unaligned-load implementations only where generated-code evidence justifies them and carefully distinguishes MSVC from clang-cl/Intel compatibility macros. Guy Harris supplied substantial architecture/compiler review.
- **!9236 — decompressed TVBuff ownership.** A Valgrind-backed audit converts several dissectors from raw tvb_uncompress results to child TVBuffs and documents the raw-result free-or-attach obligation. John Thacker explicitly recommends child helpers for the common lifetime.
- **!9234 — checker severity and rollout.** Jaap Keuter objects to describing optional true_false_string deduplication as an error; Martin Mathieson also declines to enable the new option in CI until the existing warning baseline is cleared.
- **!9226 — counterintuitive text wire order.** Guy Harris documents SRT's unusual little-endian-word/string-octet ordering and the distinction between NUL padding and NUL termination, preventing an apparently obvious endian change from altering correct behavior.
- **!9223 — generated protocol-tree values.** Alexis La Goutte requests generated marking for calculated ALP fields; the contributor applies it to all calculated values while using normal item APIs for wire-backed fields.

## Important qualified evidence

- **!9261** reinforces the ElidedLabel plain-text/rich-text boundary by escaping caller text before internal rich-text construction.
- **!9257** corroborates recursion-depth limiting and expert reporting for packet-controlled recursion.
- **!9255** shows test harnesses must use the conventional `--` terminator when a filter expression can begin with a hyphen.
- **!9252** derives SNMP conversation type from the actual packet transport rather than a fixed SNMP conversation type.
- **!9246** deliberately walks back part of the more aggressive display-filter commuting/type-inference approach introduced in !9212, so it carries greater implementation weight for that sequence.
- **!9242** merged with a guint64 printf-format portability issue later identified by Stig Bjørlykke on macOS and corrected in already-reviewed !9277; the initial formatting is negative evidence.
- **!9239** updates authoritative LCS-AP ASN.1 inputs and regenerated output together.
- **!9233** adds large Bluetooth 5.3 control-procedure work, but post-merge Joakim Andersson comments identified mask and clarity issues later addressed in !9341; the correction carries more weight than those original details.
- **!9232** fixes DECT uses of proto_tree_add_bits_item with unsupported field types, corroborating typed-item API/type-contract checking.
- **!9230** contains useful Martin Mathieson review against prematurely promoting a USB-PTP-specific masked-value representation into the shared value-string API; the contributor agreed its filter semantics were not generic enough.
- **!9229** contains Alexis La Goutte review to show reserved bytes and prefer standard tree-item APIs for directly wire-backed values.
- **!9228** is a Valgrind-driven group of uninitialized-state fixes and was explicitly marked by Gerald Combs for stable backport.
- **!9227** fixes overlapping source/destination snprintf use by formatting into temporary storage first.
- **!9222** bounds expensive packet-list sorting: columns requiring dissection are sortable only when visible rows fit a configured cache, avoiding O(N log N) redissection and reporting why sorting was declined.
- **!9221 / !9220** convert a min/max literal crash into a normal display-filter compilation failure with a regression test.
- **!9218** rejects a constant arithmetic expression during semantic checking rather than letting it reach an impossible code-generation path.
- **!9217** validates and repairs SRT UTF-8 after reconstructing its unusual word-packed representation.
- **!9212** contains high-authority Guy Harris review explicitly asking about anti-commutative operators such as subtraction. João Valverde explained that inference retry did not reorder syntax operands, then noted that some of the approach was walked back in !9246.

## Complete batch inventory

| MR | Result | Review disposition |
|---|---|---|
| !9261 | merged | ElidedLabel text boundary |
| !9260 | merged | GCC 12.1 Qt warning workaround |
| !9259 | merged | **Deep:** registered Follow Stream capability |
| !9258 | merged | macOS packaging path fix |
| !9257 | merged | TIPC recursion bound |
| !9256 | merged | conversation-index stable backport |
| !9255 | merged | dftest option parsing |
| !9254 | merged | conversation-index stable backport |
| !9253 | merged | **Deep:** Lemon vendoring policy |
| !9252 | merged | SNMP conversation transport type |
| !9251 | merged | DECT initialization fix |
| !9250 | merged | dftest debug controls |
| !9249 | merged | **Deep:** conversation index mutation correctness |
| !9248 | merged | **Deep:** ASCII invalid-byte progress |
| !9247 | merged | dftest cleanup-flow fix |
| !9246 | merged | display-filter semantic-check refinement |
| !9245 | merged | ambiguous display-filter warning |
| !9244 | merged | scanner optimization |
| !9243 | merged | **Deep:** typed checker bit APIs |
| !9242 | merged | Arista subtype; later portability correction |
| !9241 | merged | Diameter 3GPP geographical update |
| !9240 | merged | GSM geographical update |
| !9239 | merged | LCS-AP ASN.1 update |
| !9238 | merged | **Deep:** compiler identity; Guy review |
| !9237 | merged | display-filter lexer/grammar refactor |
| !9236 | merged | **Deep:** decompression TVBuff ownership |
| !9235 | merged | display-filter error-location improvement |
| !9234 | merged | **Deep:** checker severity/rollout |
| !9233 | merged | Bluetooth 5.3; later correction |
| !9232 | merged | DECT bitfield API/type repair |
| !9231 | merged | PFCP 5GS UPN correction |
| !9230 | merged | USB-PTP port; extensive review |
| !9229 | merged | sACN Universe Discovery |
| !9228 | merged | Valgrind-driven initialization fixes |
| !9227 | merged | overlapping snprintf correction |
| !9226 | merged | **Deep:** Guy Harris SRT representation documentation |
| !9225 | merged | display-filter compatible min/max types |
| !9224 | merged | ASCII batching; qualified by !9248 |
| !9223 | merged | **Deep:** ALP generated-field review |
| !9222 | merged | packet-list sort-cache bound |
| !9221 | merged | min/max literal crash backport |
| !9220 | merged | min/max literal crash fix |
| !9219 | merged | warning-policy cleanup |
| !9218 | merged | constant-expression crash prevention |
| !9217 | merged | SRT UTF-8 validation |
| !9216 | merged | min/max constant type inference |
| !9215 | merged | assertion log-level fix |
| !9214 | merged | **Deep:** UTF-8 consumed-byte accounting |
| !9213 | merged | grammar leak fix |
| !9212 | merged | Guy-reviewed arithmetic inference, later refined |
