# Wireshark MR review ledger — !9261 through !9212

Reviewed: **2026-10-31**. Corpus: `dheitmueller/wireshark-corpus-mrs`, exact commit `ddcaa22b51c68f594e425a23388c3a2086813054`. Notebook starting head: `fa190226660c1b7bca73649dc871119701698000`.

**Exact reviewed membership (descending, 50 distinct):** !9261, !9260, !9259, !9258, !9257, !9256, !9255, !9254, !9253, !9252, !9251, !9250, !9249, !9248, !9247, !9246, !9245, !9244, !9243, !9242, !9241, !9240, !9239, !9238, !9237, !9236, !9235, !9234, !9233, !9232, !9231, !9230, !9229, !9228, !9227, !9226, !9225, !9224, !9223, !9222, !9221, !9220, !9219, !9218, !9217, !9216, !9215, !9214, !9213, !9212.

**Outcomes: 50 merged, 0 closed-unmerged, 0 open.** Every entry is a historical outcome as recorded in the pinned corpus snapshot, not a new live GitLab merge-state claim.

## Selection and preservation

Inventoried all **361** existing files in `reviewed-mrs-automation/`, read the root `reviewed-mrs.md`, supplemental `reviewed-mrs-automation/reviewed-mrs-automation.md`, preceding exact !9311–!9262 ledger and all exceptional/noncontiguous/plus/skip/gap/backfill tracking files. None of the root, supplemental or exceptional ledgers records a candidate in !9261–!9212 as previously reviewed. Ordinary preceding ledger membership begins at !9262; its earlier full reconciliation identified !9261 as next candidate, and neither corpus nor notebook changed between that reconciliation and this selection. **No whole numeric range was promoted to reviewed based only on a partial ledger.** All fifty selected corpus JSON records were fetched and their surviving descriptions, discussions, merge outcomes and available diffs were analyzed. The full repo-wide per-file membership reconciliation was performed in the preceding run; this run independently inventoried the current tracking and rechecked potentially relevant exceptional files.

The untouched historical `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` was fetched and its explicit table contains **exactly 50 distinct entries**, all of !17571 through !17620 inclusive. Those 50 remain previously reviewed and excluded. No older ledger was changed.

## Exact MR assessments

| MR | Corpus state | Depth | Review evidence and disposition |
|---|---|---|---|
| !9261 | merged | Scanned | Gerald Combs makes ElidedLabel setText() HTML-escape text; later !9462 adapts callers to the plain-text contract. |
| !9260 | merged | Scanned | Narrow diagnostic suppression for GCC 12.1 stringop-overread false alarm in Qt data printer. |
| !9259 | merged | Deep | John Thacker registers optional substream-navigation callback for Follow Stream; QUIC stream 0 differs from HTTP/2. Alexis found a dead store fixed by the author. |
| !9258 | merged | Scanned | Fixes macOS ChmodBPF installer attempting to copy a nonexistent plist. |
| !9257 | merged | Deep | Gerald Combs bounds TIPC recursive dissection using per-protocol depth and expert warning; corroborates already documented stable backports !9284/!9285. |
| !9256 | merged | Scanned | release-3.6 backport of John Thacker's conversation hash-table removal fix !9249. |
| !9255 | merged | Deep | João Valverde inserts -- before filter expressions (including leading minus) in dftest tests and updates option parsing. |
| !9254 | merged | Scanned | release-4.0 backport of conversation hash-table removal fix !9249. |
| !9253 | merged | Discussion-focused | Synchronizes vendored Lemon generator with upstream, removes obsolete patches, and isolates its warning policy from project source. |
| !9252 | merged | Scanned | SNMP derives conversation type from packet transport rather than forcing CONVERSATION_SNMP. |
| !9251 | merged | Scanned | Initializes DECT-NWK local to eliminate maybe-uninitialized build warning. |
| !9250 | merged | Discussion-focused | Adds opt-in Flex/Lemon parser debugging to dftest; option-handling follow-up !9255 matters for negative-leading expressions. |
| !9249 | merged | Deep | John Thacker fixes wrong conversation hash-table removal in conversation_set_addr2; can segfault on missing lookup. Two merged stable backports. |
| !9248 | merged | Deep | John Thacker advances ASCII decoder pointer after adjacent invalid bytes; fixes regression from batched valid-byte appends. |
| !9247 | merged | Scanned | Adds missing dftest goto exit path. |
| !9246 | merged | Deep | João Valverde removes arithmetic commutation strategy and tightens type-checking; supersedes part of historical !9212. |
| !9245 | merged | Deep | Adds display-filter compilation warnings for ambiguously interpreted tokens and guidance to disambiguate with explicit syntax. |
| !9244 | merged | Deep | Lexer avoids unnecessary field/function lookups for lexically invalid names; adds ambiguity semantics and regression tests. |
| !9243 | merged | Deep | Martin Mathieson enables source validation for proto_tree_add_bits_item and proto_tree_add_bits_ret_val. |
| !9242 | merged | Discussion-focused | Arista AVSP subtype; Stig reports macOS format-width mismatch later fixed !9277; large packet-avsp.c patch missing in corpus. |
| !9241 | merged | Scanned | Updates 3GPP Diameter geographical-description fields. |
| !9240 | merged | Scanned | Updates GSM geographical-description location formats. |
| !9239 | merged | Scanned | Updates LCS-AP ASN.1 specification and conformance; generated packet-lcsap.c diff omitted from snapshot. |
| !9238 | merged | Deep | John Thacker optimizes portable unaligned integer access via compiler-gated memcpy/byteswap; Guy Harris discusses MSVC __unaligned and endian assumptions. |
| !9237 | merged | Deep | Replaces generic unparsed dfilter tokens with separate identifier/constant lexical classes and simplifies grammar error locations. |
| !9236 | merged | Deep | Fixes leaks by using tvb_child_uncompress; John Thacker clarifies unattached TVB lifetime and accurate parent ownership in API docs. |
| !9235 | merged | Scanned | Improves dfilter function error caret range to cover the whole call. |
| !9234 | merged | Deep | Martin Mathieson adds advisory value_string-to-TFS checker; Jaap Keuter requests severity downgraded from error to warning/note. Optional, not initially CI. |
| !9233 | merged | Discussion-focused | Bluetooth control procedure update; Joakim Andersson flags bit masks, comments and brace style repaired in later !9341. Do not treat initial merged version as final. |
| !9232 | merged | Deep | DECT-NWK replaces invalid proto_tree_add_bits_item field types; Martin Mathieson approves. |
| !9231 | merged | Discussion-focused | PFCP Bridge ID changed from 6-byte FT_BYTES to 8-byte FT_UINT64; Alexis questions length checks and historical capture compatibility. |
| !9230 | merged | Deep | USB-PTP import; Martin questions exporting masked value-string pseudo-type, contributor identifies broken string filter semantics and favors local formatting. Large source patches missing. |
| !9229 | merged | Discussion-focused | sACN universe discovery; Alexis requests direct tree-add APIs, reserved bytes and clear summary formatting; author adjusts submitted code. |
| !9228 | merged | Deep | Gerald Combs fixes Valgrind uninitialized paths using zero-initialized structs and buffers; backport requested. |
| !9227 | merged | Scanned | release-3.6 compiler warning fix avoids same-source/dest snprintf overlap with temporary string. |
| !9226 | merged | Scanned | Guy Harris documents SRT text wire packing: 32-bit little-endian words with zero padding, not conventional raw UTF-8. |
| !9225 | merged | Deep | João Valverde allows compatible field types, not only identical ftenums, in min()/max() argument checking. |
| !9224 | merged | Deep | John Thacker batches valid ASCII bytes for fewer allocations; consecutive-invalid correction follows at !9248. |
| !9223 | merged | Deep | ATSC ALP dissector and pcap linktype; Alexis requests generated annotations for calculated fields, direct add_item where possible, and precise filter-field names. |
| !9222 | merged | Deep | John Thacker bounds packet-list dissection-dependent sort by configurable cached-row budget to avoid O(N log N) dissections. |
| !9221 | merged | Scanned | release-4.0 backport of literal min()/max() semantic-check crash fix !9220. |
| !9220 | merged | Deep | João Valverde rejects wrong AST node kinds before field-only accessor in min()/max() to prevent crash. |
| !9219 | merged | Scanned | release-4.0 warning policy change removes -Wjump-misses-init. |
| !9218 | merged | Deep | João Valverde replaces internal assertion for untyped constant arithmetic with user-facing semantic error and regression test. |
| !9217 | merged | Deep | SRT UTF-8 validates and repairs protocol text; payload packing unusual, document decoder limitations. |
| !9216 | merged | Deep | João Valverde defers literal type resolution in min()/max() until typed operand known; temporary allocation scope protects exception cleanup. |
| !9215 | merged | Scanned | Corrects display-filter assertion log severity to error. |
| !9214 | merged | Deep | Gerald Combs fixes UTF-8 replacement bounds; John Thacker corrects remaining-length decrement to count entire validator-consumed span. |
| !9213 | merged | Scanned | Fixes Lemon parser grammar leak by freeing function-call delimiter AST nodes after merged location. |
| !9212 | merged | Deep | Exploratory dfilter arithmetic commute; Guy Harris challenges subtraction semantics, João says AST operand order preserved but later rolls back strategy in !9246. |

## Durable notebook updates and evidentiary cautions

- **Follow Stream !9259:** protocol registration advertises a substream navigation callback; QUIC and HTTP/2 IDs must not be conflated. Incorporated into `follow-stream-extension-conventions.md`.
- **Conversation !9249 (+ !9254, !9256):** remove keys from their correct partial-key hash table before rekeying. Incorporated into `conversation-api-conventions.md`.
- **TVBuff decompression !9236:** unattached decompressed TVBs are caller-owned; prefer parent-attached child helper when appropriate, as John Thacker explicitly advised. Incorporated into `tvbuff-composite-ownership-conventions.md`.
- **Checker !9234:** Jaap Keuter's severity feedback differentiates advisory shared-TFS cleanups from errors, and Martin Mathieson did not yet enable the new mode in CI. Added to `checker-tooling-conventions.md`.
- **USB-PTP !9230:** avoid masquerading a masked enum as a conventional value_string; the review identifies filter semantics broken by the pseudo-table. Added to `field-value-semantics-conventions.md`.
- **Display filter !9212, !9216, !9220, !9225, !9246:** literal inference, AST type guards, compatible types and a later rollback of commutation strategy; Guy Harris's anti-commutative-operation concern preserved. Added to `dfilter-type-inference-conventions.md` and `dfilter-conventions.md`.
- **Qt !9222:** bounded cached-column sort prevents O(N log N) packet redissections; included in `qt-model-conventions.md`.
- **Encoding !9214, !9224, !9248:** use consumed-byte spans during replacement and keep pointer progress across consecutive malformed bytes; John Thacker's direct correction has very high authority. Included in `text-encoding-conventions.md`.
- **Unaligned data !9238:** compiler-specific memcpy/byteswap implementations preserve alignment and byte order; Guy Harris's platform commentary is distinguished from an adopted API. Included in `wire-format-integer-access-conventions.md`.
- **Bitfield checking !9232/!9243:** accepted Martin Mathieson rules extend typed-item validation to bit-add APIs. Included in `source-checker-conventions.md`.
- **CLI !9255:** `--` protects filter expressions starting with minus signs from option parsing, including in the test suite. Included in `cli-option-semantics-conventions.md`.

**Evidence limits:** !9230's newly introduced large USB-PTP source/header patches and !9242's large AVSP file diff are absent from `changes_data`; the conclusions for those files rest on recorded discussion and retained other patches, not invented code inspection. !9239 similarly lacks the generated LCS-AP C diff. !9233 was merged but subsequent !9341 corrections matter; !9212 was later modified by !9246; !9242's portability warning was addressed in !9277. Merged status does not make a defective intermediate implementation authoritative. No tests, builds, fuzzing, or capture analysis were executed. All conclusions concern the pinned 2022–2023 historical corpus.

## Continuation

Next expected candidate **!9211** (present in corpus but **not counted or reviewed** in this run), subject to the next run's complete current-membership check.
