# Automated Wireshark MR review: !13313 through !13362

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook repository: `dheitmueller/wireshark-chatgpt`
- Notebook base before this run: `466baf36b65760f2f222fc0c78c387faa0e7207d`
- Review direction: descending MR number, newest available previously-unreviewed first.
- Batch size: 50 MRs.
- Status mix: 48 merged; 2 closed/unmerged (`!13357`, `!13352`).

## Selection and duplicate avoidance

Before selecting the batch, the run consulted `reviewed-mrs.md`, the aggregate automation tracking, and the per-run ledgers under `reviewed-mrs-automation/`. The already-reviewed set was built from explicitly recorded MR numbers; no numeric interval was inferred merely because neighboring MRs appeared in a ledger. The historical reviewed batch `!17571` through `!17620` was explicitly preserved/counting in the already-reviewed set. The previous ledger recorded `!13362` only as a frontier probe, not as reviewed, so it was eligible for this run.

Exact reviewed MR numbers, in review order:

- !13362
- !13361
- !13360
- !13359
- !13358
- !13357
- !13356
- !13355
- !13354
- !13353
- !13352
- !13351
- !13350
- !13349
- !13348
- !13347
- !13346
- !13345
- !13344
- !13343
- !13342
- !13341
- !13340
- !13339
- !13338
- !13337
- !13336
- !13335
- !13334
- !13333
- !13332
- !13331
- !13330
- !13329
- !13328
- !13327
- !13326
- !13325
- !13324
- !13323
- !13322
- !13321
- !13320
- !13319
- !13318
- !13317
- !13316
- !13315
- !13314
- !13313

## Durable notebook findings promoted

### Recent/history state is not active configuration

Merged master MRs !13343 and !13353, authored by John Thacker, correct remote-capture behavior where remembered rpcap hosts were implicitly treated as hosts to contact again. The accepted design separates recent-history/suggestion data from explicitly active remote hosts and stores the active configuration separately. The same run also reviewed release backports !13326-!13328 of the capture-filter history fix, which makes the MRU ordering agree with bounded persistence and avoids premature truncation before the effective limit is known.

Promoted to `recent-state-and-history-conventions.md` in commit `0c909bbc92bf7a0e2e616c4a55dad06e7faae215`.

### Cleanup APIs inherit producer-success preconditions

Merged !13315 added missing pcap cleanup, but called `pcap_freecode()` even when `pcap_compile()` failed. Merged !13362, also by John Thacker, fixes the resulting crash by freeing compiled BPF code only after compilation succeeded. The pair is strong evidence that cleanup must be paired with the exact acquisition/initialization event that makes the object valid, rather than made unconditional merely because a variable is in scope.

Promoted to `resource-cleanup-precondition-conventions.md` in commit `4003159d7441f80bb5b725145ebdc5ddc4546b9b`.

### Size formatting buffers for the formatted value, not the source integer

Merged master !13337, authored and merged by John Thacker, fixes `BASE_OUI` label truncation. Numeric-label buffers sized for ordinary integer text were too small once manufacturer-name resolution was included; the accepted change introduces a shared 80-byte numeric-label capacity.

Promoted to `formatted-label-buffer-conventions.md` in commit `e32d70c850b6f716fea2d15ed03eec5af2b495b0`.

### Preserve “validation unavailable” as distinct from “input invalid”

Merged master !13317, with release backports !13323-!13325, stops trying to compile capture filters when the selected interface has an unknown DLT (`active_dlt == -1`). The UI now reports that the filter cannot be checked instead of producing a misleading validation error.

Promoted to `validation-availability-conventions.md` in commit `27c9ef6b2724d86cad6ca9ee16bad2e18e485432`.

## Strong corroboration retained without duplicate rules

- !13313, authored and merged by Guy Harris, adds BLF length/header validation, error propagation from the scan helper, cleanup on failed open, and checks every `file_seek()` result. Together with Guy's merged !13332 and !13335, this strongly corroborates the existing `wiretap-reader-validation-conventions.md` rule to validate length relationships before seeking/deriving sizes and to propagate real parser/open failures instead of continuing with partial state. No duplicate rule was added.
- !13316 reinforces the existing review-scope rule: Stig Bjørlykke explicitly declined to mix uncertain unused-field cleanup into a mechanical `hf_` rename because deciding whether the entries should be removed or used required separate semantic investigation. The MR merged with the focused rename.
- !13318 reinforces the source-checker/tooling rule that automated transformations/checkers have semantic limits: `checkhf.py` did not understand one array use, and the author backed out the associated rename rather than forcing the checker result into a questionable change.
- !13329 moves a fast, deterministic help-URL integrity check into the early Commit Check path, corroborating the practice of catching cheap structural repository/UI-documentation errors before expensive build jobs.
- !13333 and !13334 remove Debian Lintian enforcement from upstream CI on the explicit rationale that Wireshark supports downstream distributions but is not exclusively a Debian package; distribution-specific policy enforcement needs a clear upstream ownership/maintenance commitment.
- !13338, authored and merged by Guy Harris, documents empirically observed BLF padding/layout behavior rather than pretending an uncertain format property is a universal alignment rule.
- !13356 is a focused display-filter lexer correction that narrows byte-literal hyphen recognition so ordinary arithmetic such as `64-63` remains subtraction, corroborating context-sensitive lexer guidance already present in the notebook.
- !13357 and !13352 were closed/unmerged and were therefore down-weighted relative to accepted implementation evidence.

## Frontier check

After reviewing the 50 selected MRs, `mr_13312.json` was fetched only to verify that the descending corpus continues. `!13312` exists and is merged; it was **not** counted as reviewed in this run. The corpus therefore has not run out, and no scraper-restart notification is required. Absent newly scraped higher-numbered unreviewed material, `!13312` is the next descending candidate.
