# Wireshark MR Review Automation Ledger — !24359–!24408

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers under `reviewed-mrs-automation/`, using individual MR entries rather than assuming that a numeric range was wholly reviewed. The historical !17571–!17620 batch remains explicitly preserved and counted. With the corpus commit unchanged from the previous run and !24409–!24458 explicitly reviewed, the fifty highest-numbered corpus MRs absent from the reconstructed reviewed set are exactly !24408 through !24359.

Exact reviewed MR set, newest to oldest:

!24408, !24407, !24406, !24405, !24404, !24403, !24402, !24401, !24400, !24399,
!24398, !24397, !24396, !24395, !24394, !24393, !24392, !24391, !24390, !24389,
!24388, !24387, !24386, !24385, !24384, !24383, !24382, !24381, !24380, !24379,
!24378, !24377, !24376, !24375, !24374, !24373, !24372, !24371, !24370, !24369,
!24368, !24367, !24366, !24365, !24364, !24363, !24362, !24361, !24360, !24359.

| MR | Review result |
|---|---|
| !24408 | Merged; WSLua debugger unsaved-title indicator. Straightforward Qt UI state presentation; no new durable convention. |
| !24407 | Merged; debugger Locals/Upvalues follow selected Lua stack frame with synchronized debugger state. Useful implementation context, no broader rule promoted. |
| !24406 | Merged by Jaap Keuter; adds RTPS SM2/SM3/SM4 vendor algorithm recognition. Author supplied a concrete capture/tshark validation path; protocol data update rather than a new coding convention. |
| !24405 | Merged release-4.6 backport of wsutil xxhash-private linkage; reinforces !24393's implementation-private dependency rule. |
| !24404 | Merged release-4.6 backport linking `wmem_test` against `wsutil_static`; reinforces !24393's internal-test linkage rule. |
| !24403 | Merged release-4.6 backport adding native Arm64 Asciidoctor bundle on macOS as Rosetta dependency is removed. Build-platform maintenance. |
| !24402 | Merged release-4.6 warning-control typo fix restoring the intended cast-qual diagnostic state. Compiler-hygiene maintenance. |
| !24401 | Closed/unmerged backport removing duplicate include directories. Down-weighted because it was not accepted on the target branch. |
| !24400 | Merged John Thacker Coverity cleanup making NDPS alignment arithmetic unsigned consistently. Corroborates existing type/arithmetic guidance. |
| !24399 | Merged master Arm64 Asciidoctor bundle support for macOS. Platform/tooling maintenance. |
| !24398 | Merged by Jaap Keuter; RTPS participant-security flag masks corrected and field naming aligned with siblings. Author supplied a one-frame validation capture; protocol correctness, no broader rule promoted. |
| !24397 | Merged; factors detailed Bluetooth BR/EDR channel-map decoding into a common dissector used by HCI commands/events. Reinforces shared-decoder reuse. |
| !24396 | Merged release-notes typo fix. No durable engineering convention. |
| !24395 | Merged release-4.6 release-note clarification for extcap binary relocation to libexec. User-facing migration documentation, but no new coding rule. |
| !24394 | Deep review; merged by Jaap Keuter. Fixes multi-fragment RTPS source offsets and resets extended parameter-length width per iteration. Promoted record-local parser-state/reset guidance; capture evidence identified concrete successful reassemblies. |
| !24393 | Deep review; merged master by John Thacker. Makes xxhash private to wsutil and links `wmem_test` to `wsutil_static`. Jaap Keuter confirmed the test arrangement and Guy Harris explicitly corroborated the internal-unit-test/static-library pattern via libpcap. Promoted with highest weighting. |
| !24392 | Merged release-4.4 backport preferring `tvb_memdup()` over allocate-then-`tvb_memcpy()` for packet-derived lengths. Reinforces !24387/!24390. |
| !24391 | Merged release-4.6 backport of the same `tvb_memdup()` safety change. |
| !24390 | Deep review; merged master and authored by John Thacker. Adds explicit API documentation recommending `tvb_memdup()` when possible because bounds are checked before allocation. Promoted as durable TVB copy/allocation guidance. |
| !24389 | Merged WSLua debugger editable-script editor with save/discard/cancel handling. Feature implementation; no separate reusable convention promoted. |
| !24388 | Merged master release-note clarification for extcap libexec move. Documentation maintenance. |
| !24387 | Deep review; merged master by John Thacker. Converts RTPS copy paths to `tvb_memdup()` so packet bounds are checked before allocating packet-sized memory. Promoted with !24390 and stable backports. |
| !24386 | Merged master; adds another overflow check to `wmem_array_grow`. Reinforces existing checked-arithmetic/allocation-hardening guidance. |
| !24385 | Merged master; LZ* decompression now checks `wmem_array_grow()` failure and stops cleanly. Reinforces allocation failure propagation. |
| !24384 | Merged release-4.4 backport checking failure to parse a WSLua GUID literal. Defensive conversion/error handling. |
| !24383 | Merged release-4.6 backport of WSLua GUID conversion failure handling. |
| !24382 | Deep review; merged after a broad Doxygen expansion over 44 files. Author disclosed that documentation was mostly AI-generated and manually reviewed/corrected. Retained as submission/review evidence but not promoted as a new coding convention because the discussion did not establish a stronger reusable rule. |
| !24381 | Merged master WSLua GUID conversion failure check. Straightforward validation fix. |
| !24380 | Merged release-4.4 ICMPv6 NDP padding fix using absolute rather than relative offset. Backport corroboration. |
| !24379 | Merged release-4.6 backport of the ICMPv6 padding fix. |
| !24378 | Deep review; merged master authored/merged by John Thacker. Applies CMake's one-producer rule for generated files: one custom target owns generation and parallel consumers depend on it. Promoted by strengthening generated-resource dependency guidance. |
| !24377 | Merged WSLua debugger Coverity fixes synchronizing shared debugger fields under the mutex and removing impossible UI null checks. Reinforces synchronization discipline. |
| !24376 | Merged master ICMPv6 NDP padding correction. Protocol-offset correctness; no separate new rule. |
| !24375 | Merged WSLua debugger Step Over/In/Out support. Feature work without durable review guidance. |
| !24374 | Merged release-4.4 backport bounding total SMB2 chained decompression and ceasing expensive decompression after terminal error. Reinforces resource-limit guidance. |
| !24373 | Merged release-4.6 backport of SMB2 chained-decompression bounds. |
| !24372 | Merged master LZ* decompression optimization: pre-grow once, bulk-copy when possible, and avoid awkward signed-negation arithmetic. Performance/type-safety corroboration. |
| !24371 | Merged master; `wmem_array` growth APIs return failure explicitly. Reinforces existing must-use/allocation-result guidance; no duplicate rule added. |
| !24370 | Merged release-4.4 backport of SMB2 PATTERN_V1 decompression optimization (~50x on large input). Performance maintenance. |
| !24369 | Merged release-4.6 backport of the same decompression optimization. |
| !24368 | Merged release-4.4 WebSocket zlib output bound, preserving stateful sliding-window behavior while limiting decompression. Resource/DoS bound corroboration. |
| !24367 | Merged release-4.6 backport of WebSocket zlib output bound. |
| !24366 | Merged master SMB2 total chained-compression bound; after a terminal failure it keeps useful tree fields but stops decompression work whose result will be discarded. Strong corroboration of bounded expensive parsing. |
| !24365 | Merged release-4.4 backport removing GSM RP dependence on a global protocol tree and deriving the parent tree from the passed context. Reinforces reentrant/direct-call safety. |
| !24364 | Merged release-4.6 backport of the GSM RP global-state removal. |
| !24363 | Merged master RPM spec compatibility fixes for older RPM macro/expression implementations. Packaging portability maintenance. |
| !24362 | Merged master SMB2 PATTERN_V1 decompression optimization. Performance maintenance with existing samples exercising the algorithms. |
| !24361 | Deep review; merged by Anders Broman. Bounds optional RTPS user-data subdissection to the exact serialized payload and converts nested failure on valid encrypted/opaque data into localized expert warning rather than malformed outer RTPS. Promoted as a narrow optional-dispatch exception-boundary rule, with supplied capture validation showing no regression on cleartext traffic. |
| !24360 | Merged RTPS NACK_FRAG bitmap display off-by-one fix, matching already-correct sibling logic. Straightforward presentation correctness. |
| !24359 | Deep review; merged by Anders Broman. Accumulates a `handled` result across standard and vendor-specific RTPS parameter decoders and emits raw fallback bytes only after all decoders decline. Capture validation confirms decoded vendor PIDs no longer get redundant raw data while unknown PIDs retain fallback bytes. Reinforces successful-claim/fallback dispatch guidance. |

Promoted durable findings in this run:

1. !24387/!24390 with !24391/!24392: prefer `tvb_memdup()` for newly owned copies of contiguous packet data when possible so range validation occurs before packet-sized allocation.
2. !24393 with !24404/!24405: keep implementation-only dependencies private, and let internal unit tests link the appropriate static/internal target rather than widening the public ABI; Guy Harris explicitly corroborated this model.
3. !24378: one generated output should have one owning CMake generation rule/target; all parallel consumers depend on that producer.
4. !24361: at an optional/speculative nested-dissection boundary where payload may validly be opaque, use an exact bounded sub-TVB and localize child decode failure without suppressing genuine outer-protocol truncation.
5. !24394: reset record-local parser modes inside each iteration; special extended-width state must not leak into the next record, and per-fragment source offsets must advance with each fragment.

Closed !24401 was explicitly down-weighted. Merged MRs and stable backports were weighted more strongly; substantive maintainer evidence received extra weight, with Guy Harris's !24393 comment treated as particularly authoritative.