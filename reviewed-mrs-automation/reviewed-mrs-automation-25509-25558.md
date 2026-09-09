# Wireshark MR automation review: !25509–!25558

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Review direction: newest to oldest
- Reviewed in this run: exactly 50 previously unreviewed MRs, !25558 through !25509 inclusive
- Selection method: consulted existing per-run ledgers and `reviewed-mrs.md` where applicable, built the already-reviewed set, and selected the 50 highest-numbered corpus MRs not already recorded as reviewed.

| MR | Review depth | Outcome and durable evidence |
|---|---|---|
| !25558 | Deep | Merged, authored and merged by Guy Harris. Windows Search names retained beyond packet dissection are moved to file scope; display strings use display-safe extraction and related allocations are zero-filled. Strong lifetime/scope exemplar; reinforces existing allocator-scope guidance. |
| !25557 | Deep | Merged SSH keylog hardening. Guy Harris asked whether malformed keylog entries should become SSH expert info; John Thacker noted configured-file/DSB errors cannot safely be attached to a packet without evidence tying the key to that session. Promoted diagnostic-association guidance. |
| !25556 | Scanned | Merged O-RAN FH CUS change replacing section-info storage with a `wmem_tree`. No substantive human correction or distinct new convention. |
| !25555 | Scanned | Merged release-4.4 backport of the `strptime` terminal-NUL bounds fix represented by !25551. No additional lesson. |
| !25554 | Scanned | Merged release-4.6 backport of !25551. No additional lesson. |
| !25553 | Scanned | Merged release-4.4 backport of the LZ4 uninitialized-stream flush/close guard represented by !25542. |
| !25552 | Discussion-focused (open draft) | Draft change to reject date-only time literals. Existing tests deliberately exercised date-only input and the proposal remained open; treat semantics as provisional, not an accepted behavior rule. |
| !25551 | Deep | Merged master `strptime` fix checks for terminal NUL before examining the following character, eliminating a one-byte overread. Strong boundary-short-circuit exemplar, but already covered by existing parser/bounds guidance. |
| !25550 | Discussion-focused (open) | Falco raw-block work remained dependent on upstream falcosecurity/libsinsp changes and was iterated with a placeholder dependency version. Useful dependency-development context, but not accepted architecture evidence yet. |
| !25549 | Discussion-focused | Merged TCP duplicate-detection follow-up. Guy Harris corrected GitLab reference syntax: `!` denotes merge requests; commit IDs/commit-comment links should be referenced as commits rather than written as MR IDs. Mostly submission/tooling-specific. |
| !25548 | Deep | Merged JSON-output hot-path optimization supplied a large-capture benchmark and `perf` profile, quantifying wall-clock and cycle reductions. Good performance-evidence exemplar; no separate coding convention promoted. |
| !25547 | Deep | Merged IEEE 802.15.4 CCM stack-underflow repair. A warning-driven cleanup accidentally changed array indices/variables while changing style; fix preserved the intended style change and validated OSCORE, 802.15.4, ZigBee, build warnings, and cppcheck. Promoted static-analysis-change regression guidance. |
| !25546 | Scanned | Merged release-4.6 backport of the LZ4 uninitialized-stream lifecycle guard represented by !25542. |
| !25545 | Scanned | Merged capture-options cleanup reusing `test_for_fifo()` from libwsutil instead of copying its implementation. Reinforces existing shared-helper reuse guidance. |
| !25544 | Deep | Merged dumpcap pcapng-passthrough fix stops flushing after every packet because it destroys compression ratio; flushes remain at synchronization/notification boundaries. Strong buffered-I/O design evidence; recorded here without a new dedicated notebook rule. |
| !25543 | Scanned | Merged LZ4 comment correction reflecting existing linked-block fast seeking. Documentation-only; no durable review lesson. |
| !25542 | Deep | Merged master LZ4 lifecycle fix. Flush/finalization is skipped when compression state was never initialized, while the underlying file still closes. Reinforces initialization-state-aware cleanup/finalization. |
| !25541 | Deep | Merged pcap packet-data fix temporarily changes `pkt_encap` for a nested dissector that needs inner encapsulation, then restores the outer record value before returning so later wiretap output sees consistent metadata. Promoted scoped shared-context mutation guidance. |
| !25540 | Scanned | Merged BLF buffer-limit reduction to a still-large realistic maximum so Coverity no longer treats the allocation bound as suspicious. No distinct new convention. |
| !25539 | Scanned | Merged zlib unlikely-error-path leak cleanup and readability improvement. Reinforces ordinary cleanup discipline. |
| !25538 | Deep | Merged 64-bit `time_t`/Y2038 build option. The MR explicitly requires libraries exposing `time_t` in their ABI (notably libpcap) to use the same representation; Gerald Combs also challenged an irrelevant Win32-only check, which was removed because 32-bit Windows is unsupported. Promoted ABI-affecting build-option guidance. |
| !25537 | Scanned | Merged WLAN block-ack fix models the sequence number as its actual 12-bit protocol value and uses normal typed tree addition. Reinforces field-semantic accuracy. |
| !25536 | Scanned | Merged Qt filtering-loop fix for a line omitted from the preceding change. Useful reminder to verify all intended working-tree changes are included, but no new durable rule. |
| !25535 | Scanned | Merged dissector warning cleanup. No substantive human review in the corpus snapshot warranting a new rule. |
| !25534 | Discussion-focused (open draft) | Guy Harris draft refactoring compressed-file writing, error propagation, and thread-local error buffers. High-authority design work but still open; kept as provisional context rather than accepted architecture. |
| !25533 | Scanned | Merged BVLC Clang Analyzer dead-store cleanup. No additional lesson beyond existing static-analysis workflow guidance. |
| !25532 | Deep | Merged LBMC tvbuff leak fix uses `tvb_new_child_real_data()` so dynamically created tvbuff data participates in the parent destruction chain. Strong ownership-API exemplar; reinforces existing lifetime guidance. |
| !25531 | Scanned | Merged Zebra uninitialized-memory fix for a copy/paste error. No distinct reusable convention. |
| !25530 | Scanned | Merged AFP 2.x ProDOS Info Block support uses bounded protocol-aware heuristics to distinguish a flag later reused for UTF-8 offset semantics. Protocol-specific compatibility handling; no general rule promoted. |
| !25529 | Deep | Merged first-contribution MacIP dissector, ultimately merged by Guy Harris. Guy removed an unused/colliding `hf_macip` declaration and required redundant field blurbs to be NULL or genuinely more descriptive than the field name. Promoted `hf_`/blurb registration guidance. |
| !25528 | Deep | Merged EBHSCR tvbuff leak fix anchors the synthetic header tvbuff as a child of the main tvbuff so it is destroyed with its owner. Corroborates !25532 ownership-chain guidance. |
| !25527 | Scanned | Merged release-4.6 zlib const-compatibility backport. No additional lesson. |
| !25526 | Scanned | Merged release-4.6 BLF follow-up ensuring `inflateEnd()` is called. Backport/fixup; no additional lesson. |
| !25525 | Scanned | Merged master zlib compatibility fix accounts for historical `z_stream.next_in` constness differences between zlib builds. Narrow compatibility detail. |
| !25524 | Scanned | Merged release-4.4 BLF decompression hardening: validates actual versus claimed output size, rejects impossible theoretical sizes, and bounds initial allocation growth. Reinforces existing hostile-length/decompression bounds guidance. |
| !25523 | Scanned (closed/superseded) | Closed unmerged release-4.4 BLF backport superseded by the successful backport !25524. Low evidentiary weight. |
| !25522 | Scanned | Merged release-4.6 BLF decompression hardening corresponding to !25524. No additional lesson. |
| !25521 | Scanned | Merged BLF follow-up adds missing `inflateEnd()` cleanup. Reinforces normal library-lifecycle cleanup. |
| !25520 | Scanned | Merged release-4.6 Qt layout fix preventing capture-filter controls from overlapping when their parent is allowed below the child layout's required size. UI-specific. |
| !25519 | Scanned | Merged LZ4 additional error check for an unlikely library failure. Reinforces checking library return contracts. |
| !25518 | Scanned | Merged zlib cleanup puts a small stream object on the stack to eliminate a separate heap-free obligation. Useful ownership simplification, but already covered by lifetime/cleanup principles. |
| !25517 | Deep | Merged B.A.T.M.A.N. Advanced fix replaces `-1` sentinel use after offsets became unsigned with `tvb_reported_length(tvb)` as the stop value. Strong corroboration of the semantic-integer-domain/sentinel rule already promoted in the preceding review batch. |
| !25516 | Scanned | Merged O-RAN timing-delta arithmetic change avoids potential overflow. Reinforces existing arithmetic-safety guidance. |
| !25515 | Scanned | Merged TLS Coverity cleanup moves a NULL check before the dereference it is intended to guard. Basic defensive ordering; no new convention. |
| !25514 | Deep | Merged USB change removes eager `memdup` of a finalized composite tvbuff because the composite abstraction already materializes flat bytes lazily when cross-component access requires it. Good abstraction/lazy-copy exemplar; no separate rule promoted. |
| !25513 | Deep | Merged zlib error-path use-after-free fix moves cleanup to a common label and frees the in-place stream buffer only on the correct error path. Reinforces error-path ownership discipline. |
| !25512 | Deep | Merged Qt display-filter routing makes the main display-filter entry the single path for capture-wide filters so history and side effects remain consistent; internal application is moved behind the main-window connection. Strong single-authoritative-mutation-path architecture evidence. |
| !25511 | Scanned | Merged cppcheck warning cleanup. No distinct human-review lesson. |
| !25510 | Scanned | Merged DVB-S2 field-registration correction gives two distinct fields unique display/filter names and displays waveform IDs in the standard's decimal representation. Reinforces field semantic/registration accuracy. |
| !25509 | Deep | Merged, authored and merged by Guy Harris. Compression-library failures are represented as `FILE_ERR_CANT_COMPRESS` rather than mislabeled generic write failures. Promoted layer-specific error-domain semantics. |

## Notebook promotions from this run

- `dissector-context-flow-conventions.md`: scoped restoration of temporarily mutated shared dissection metadata (!25541); avoid attaching diagnostics to a packet/protocol tree without evidence associating the external/configuration error with that packet/session (!25557).
- `testing-fuzzing.md`: static-analysis/warning cleanup must remain behavior-preserving and should regression-test affected callers/edge cases; !25547 is a concrete severe counterexample.
- `dissector-conventions.md`: high-authority Guy Harris guidance from !25529 on removing unused/colliding `hf_` declarations and using NULL rather than a redundant field blurb.
- `build-conventions.md`: ABI-changing build options such as `time_t` width require all linked ABI-exposing dependencies to agree (!25538).
- `capture-diagnostic-conventions.md`: preserve the semantic layer of failures; compression failures should not be flattened into generic write failures (!25509).
