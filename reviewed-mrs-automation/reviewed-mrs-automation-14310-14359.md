# Wireshark MR automation ledger: !14310–!14359

- Corpus commit reviewed: `1c7ad509887ee25079a7865cc62ba18cba06f49a`
- Notebook starting commit used for reviewed-set reconciliation: `e013fe889393987030e1ec414a358a6c2dfbe814`
- Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the aggregate automation ledger, and the complete available `reviewed-mrs-automation/` per-run ledger inventory at the starting notebook commit. The historical !17571–!17620 batch was explicitly preserved/counting. Because the corpus commit and review-tracking state were unchanged from the immediately preceding exact !14360–!14409 run, exact MR-number subtraction selected the 50 highest-numbered remaining corpus entries: !14359 through !14310 inclusive.
- Outcome: 50 MRs reviewed; all 50 are merged. Merged master work was weighted more strongly than release backports; high-authority maintainer evidence, especially Guy Harris, was weighted according to `reviewer-authority.md`.

## Exact reviewed MRs

| MR | State | Review | Notes |
|---|---|---|---|
| !14359 | merged | Corroboration | Release-4.2 TCP Timestamp Option tree-item location fix; corroborates master !14349 and correct proto-item ownership. |
| !14358 | merged | Scan | Automatic data/update maintenance; no new durable engineering convention. |
| !14357 | merged | Scan | Automatic data/update maintenance; no new durable engineering convention. |
| !14356 | merged | Scan | Automatic data/update maintenance; no new durable engineering convention. |
| !14355 | merged | Scan | Automatic data/update maintenance; no new durable engineering convention. |
| !14354 | merged | Deep | Guy Harris-authored BLF fix: if no log container can be found, `blf_pull_next_logcontainer()` must return failure rather than assuming an element exists. Strong defensive wiretap/container invariant, but adequately covered by existing bounds/state guidance. |
| !14353 | merged | Deep/promoted | Guy Harris questioned cosmetic changes to ASN.1 copied from official specifications; accepted resolution distinguished authoritative copied material from Wireshark-owned preambles. Promoted to `authoritative-source-preservation-conventions.md`. |
| !14352 | merged | Deep/corroboration | Stable backport of progressive SocketCAN CAN XL presentation: append summary text only after the corresponding value has been decoded. |
| !14351 | merged | Deep/promoted | Guy Harris-authored master change builds Info/top-level item text progressively so truncated frames retain earlier valid presentation without prefetching later fields. Promoted to `truncated-packet-presentation-conventions.md`. |
| !14350 | merged | Corroboration | Stable backport moving SocketCAN appended text until after the value is placed in the tree; reinforces truncated-packet presentation sequencing. |
| !14349 | merged | Corroboration | Master TCP Timestamp Option fix uses the correct item for the TSVal subtree rather than a reused top-level option item; release !14359 corroborates. |
| !14348 | merged | Deep/corroboration | Guy Harris-authored master SocketCAN change appends text only after tree/value extraction; independently reinforces progressive presentation. |
| !14347 | merged | Deep/corroboration | John Thacker display-filter VM change returns final fvalues and distinguishes a valid empty value array from `NULL`; reinforces existing empty-value API guidance. |
| !14346 | merged | Scan/corroboration | Stable SocketCAN formatting cleanup uses unsigned formatting for unsigned values. |
| !14345 | merged | Scan/corroboration | Master SocketCAN formatting cleanup uses `%u` for unsigned values; type/format consistency. |
| !14344 | merged | Scan/corroboration | Stable SocketCAN summary formatting shows all meaningful ID digits and uses unsigned length formatting. |
| !14343 | merged | Scan/corroboration | Master SocketCAN summary formatting shows protocol-appropriate fixed-width hexadecimal IDs. |
| !14342 | merged | Scan | Stable SocketCAN variable-name cleanup makes combined flags/ID semantics explicit. |
| !14341 | merged | Scan | Master SocketCAN variable-name cleanup; readability/semantic naming only. |
| !14340 | merged | Corroboration | Stable CAN XL SDU-type dissector-table backport. |
| !14339 | merged | Deep/corroboration | Guy Harris-authored master CAN XL SDU-type dissector table uses typed table dispatch and named SDU constants; reinforces framework-owned dispatch rather than protocol-local switch logic. |
| !14338 | merged | Corroboration | Stable SocketCAN cleanup/widening for CAN XL length. |
| !14337 | merged | Deep/corroboration | John Thacker changes hidden-column persistence identity from non-unique format to column index. This is the implementation foundation later handled compatibly by already-reviewed !14457, which remains the stronger migration precedent. |
| !14336 | merged | Scan | Stable GTP constification moves large read-only tables into read-only storage. |
| !14335 | merged | Scan/corroboration | Stable DCM constification includes regenerated generated output; reinforces existing generator/source-of-truth practice. |
| !14334 | merged | Scan | Stable tooling constification for PCI-ID data; no new cross-cutting rule. |
| !14333 | merged | Corroboration | Reuses a common `true_false_string` found by `check_tfs.py --common`; reinforces repository checkers for repeated declarative data. |
| !14332 | merged | Deep | New AT-RL dissector revised after Jaap Keuter review to use address helpers, expose padding instead of skipping bytes, and return the consumed frame length. Strong review evidence; retained here rather than creating a new rule from a single protocol submission. |
| !14331 | merged | Corroboration | Guy Harris adds references for implementing “show file in folder” via system APIs instead of spawning programs; supports later !14417 native-API convention. |
| !14330 | merged | Deep/promoted | John Thacker custom-column work replaces regex/subset interpretation with the display-filter compiler, while explicitly preserving legacy multifield syntax and documenting context-dependent exclusions. Promoted to `shared-language-parser-conventions.md`. |
| !14329 | merged | Corroboration | Stats-tree speedup avoids resizing columns on every packet; reinforces existing UI hot-path/coalescing performance guidance. |
| !14328 | merged | Deep/corroboration | Gerald Combs-authored Falco bridge fix validates/sanitizes externally supplied strings into UTF-8 while preserving raw identity bytes for lookup; reinforces existing string/encoding guidance. |
| !14327 | merged | Scan | Master GTP constification of large read-only data structures; paired with stable !14336. |
| !14326 | merged | Deep/promoted | DBus “Show in Folder” implementation. Guy Harris clarified that functional testing is required on the platform using changed code, while unaffected macOS/Windows paths need build coverage. Promoted to `platform-conditional-testing-conventions.md`. |
| !14325 | merged | Scan/corroboration | Master SocketCAN spelling cleanup and CAN XL length widening; stable !14338 corroborates. |
| !14324 | merged | Scan | Falco bridge container I/O statistics feature; no durable cross-cutting convention extracted. |
| !14323 | merged | Scan | Falco bridge skips additional syscall fields; targeted integration maintenance. |
| !14322 | merged | Deep/corroboration | Stable backport of SocketCAN CAN XL/libpcap compatibility work from !14320. |
| !14321 | merged | Corroboration | Stable SocketCAN tree-structure fix for CAN FD hidden CAN protocol item; protocol-tree integration maintenance. |
| !14320 | merged | Deep/promoted | Guy Harris-authored master CAN XL support explicitly keeps `LINKTYPE_CAN_SOCKETCAN` fields in a canonical little-endian on-disk representation instead of creating a host-endian capture format; also carries a narrowly scoped libpcap compatibility workaround. Promoted to `capture-link-layer-byte-order-conventions.md`. |
| !14319 | merged | Corroboration | Release backport of FilterExpressionToolBar context-menu coordinate fix. |
| !14318 | merged | Corroboration | Release backport of FilterExpressionToolBar context-menu coordinate fix. |
| !14317 | merged | Corroboration | Release backport of FilterExpressionToolBar context-menu coordinate fix. |
| !14316 | merged | Deep/corroboration | Recent-column persistence stops using format metadata as identity; part of the sequence leading to index-based column identity and later compatibility migration. |
| !14315 | merged | Scan | Adds NFSv4.1 GET_DIR_DELEGATION support and separates previously overloaded notification field names; protocol feature, no new general rule. |
| !14314 | merged | Deep/corroboration | Keeps recent column width synchronized with preferences; part of the column-preference/state ownership sequence already captured by stronger later MRs. |
| !14313 | merged | Deep/down-weighted | Adds a `tryLock` guard around interface scanning. John Thacker explicitly described it as a blind attempt that did not reproduce/fix the original report; Peter Wu approved it as safe/no-deadlock. Retained as historical evidence but not promoted as causal guidance. |
| !14312 | merged | Scan | Art-Net fields/revision update; protocol-specific maintenance. |
| !14311 | merged | Deep/corroboration | John Thacker adds width/alignment editing to column preferences and notes that column indices, not format, must be the stable identity for later state synchronization. Strongly corroborated by !14337 and the later !14457 migration. |
| !14310 | merged | Scan | Gerald Combs-authored master FilterExpressionToolBar context-menu coordinate fix; release backports !14317–!14319 corroborate. |

## Durable notebook changes from this run

- `truncated-packet-presentation-conventions.md` — progressive tree/column presentation after successful decode, primarily !14351 with !14348/!14350/!14352 corroboration.
- `authoritative-source-preservation-conventions.md` — distinguish externally copied specification text from Wireshark-maintained wrapper/preamble material before spelling/style cleanup, from !14353.
- `capture-link-layer-byte-order-conventions.md` — persistent capture pseudo-headers must use a canonical byte order rather than producer-host native byte order, from !14320/!14322.
- `platform-conditional-testing-conventions.md` — functionally test changed platform-specific paths on their target platform while requiring build compatibility on unaffected platforms, from !14326.
- `shared-language-parser-conventions.md` — reuse the canonical display-filter compiler/parser when another feature exposes display-filter expressions, while handling context restrictions and legacy syntax explicitly, from !14330.

## Frontier

`mr_14309.json` exists in the same corpus commit and is merged, so the corpus is not exhausted. Absent newly scraped higher-numbered unreviewed MRs, !14309 is the next descending candidate.
