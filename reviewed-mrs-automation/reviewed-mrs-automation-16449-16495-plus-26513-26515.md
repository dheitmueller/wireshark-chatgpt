# Wireshark MR automation review ledger — !26515–!26513 and !16495–!16449

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook base commit before this run: `a21a4177159146f7fa45884f12b8d7034994cdc9`

Selection method: enumerate the complete recursive Git tree for the corpus commit, reconstruct the already-reviewed set from all available files under `reviewed-mrs-automation/` plus `reviewed-mrs.md` where applicable, explicitly preserve/count the historical !17571–!17620 batch, subtract the reviewed set from the corpus MR set, sort numerically descending, and take at most 50. No numeric range was assumed reviewed merely because neighboring MRs appeared in a ledger.

This run reviewed exactly 50 previously unreviewed MRs:

- !26515
- !26514
- !26513
- !16495
- !16494
- !16493
- !16492
- !16491
- !16490
- !16489
- !16488
- !16487
- !16486
- !16485
- !16484
- !16483
- !16482
- !16481
- !16480
- !16479
- !16478
- !16477
- !16476
- !16475
- !16474
- !16473
- !16472
- !16471
- !16470
- !16469
- !16468
- !16467
- !16466
- !16465
- !16464
- !16463
- !16462
- !16461
- !16460
- !16459
- !16458
- !16457
- !16456
- !16455
- !16454
- !16453
- !16452
- !16451
- !16450
- !16449

## Review weighting and durable findings

Merged master MRs were weighted most heavily. Stable-branch backports were used primarily as corroboration of accepted master behavior. Open/unmerged !26515 and !26514 were reviewed but down-weighted as precedent.

- **!16471 — merged master; strong durable type/format guidance.** Gerald Combs explicitly moved 64-bit integer formatting from GLib `G_GUINT64_FORMAT` / `G_GINT64_FORMAT` to standard `<inttypes.h>` `PRIu64` / `PRId64` and recommended the C99 forms. The large merged C99 conversion series in this batch (!16449-!16457, !16464-!16468, !16483-!16487) strongly corroborates the project-wide migration toward standard C types. Promoted to `c-type-conventions.md`.
- **!16450 + !16458 + !16459 — merged master; strong API-boundary correction.** Stig Bjørlykke explicitly requested preserving `gboolean` semantics for GLib APIs and stated that GLib functions using `gboolean` should keep `TRUE`/`FALSE`. !16458 and !16459 then restore `TRUE` in `g_string_free()` calls that a bulk conversion had changed to `true`. Promoted as an API-aware bulk-conversion rule in `c-type-conventions.md`; this independently corroborates the notebook's later dependency-boundary guidance.
- **!16463 — merged master; strong TCP framing/recovery guidance.** John Thacker explains that once inside `tcp_dissect_pdus()`, contradictory duplicate length/type fields cannot simply cause packet rejection. When neither duplicate can be trusted, report expert errors, consume a bounded remainder, and attempt synchronization on a later PDU. Promoted to `tcp-desegmentation-conventions.md`.
- **!16461 and !16460 — merged stable backports; strong corroboration of heuristic bounds safety.** They replace unsigned subtraction-based remaining-length checks, which can wrap, with `tvb_bytes_exist()` / `tvb_captured_length_remaining()` so heuristic probes do not throw on short input. No new notebook rule was needed because existing TVBuff/arithmetic/heuristic guidance already covers the invariant.
- **!16482 — merged master; compiler-warning portability corroboration.** A logically initialized 802.11 SNR value was restructured to silence a false-positive maybe-uninitialized diagnostic under an optimizing compiler. Useful evidence that supported-toolchain warnings matter even when control-flow reasoning says a path is impossible; no separate notebook rule added.
- **!16473 and !16469 — merged master; portability/style corroboration.** Stig Bjørlykke notes that `break` after `return` is unreachable and some compilers (including Solaris tooling in !16473) warn about it. The dedicated cleanup merged; treated as small corroborating style evidence rather than a new standalone convention.
- **!26513 — merged release-4.4 backport.** John Thacker clamps a count to the capacity of its fixed-size DDI array and checks before indexing. This corroborates the count-plus-fixed-array rule already captured from the master fix in `c-array-size-conventions.md`.
- **!26514 — open/unmerged.** Includes a sample capture and proposes delaying lower-layer data dissection while InfiniBand/SMBdirect reassembly is incomplete. Useful design/capture evidence but not treated as accepted precedent.
- **!26515 — open/unmerged.** Adds SMB2 RDMA transform handling for signed/encrypted offload. Reviewed for context but down-weighted because it remains open.

Notebook convention commits made during this run before this ledger:

- `6be8d334ac9f454990d7da35a03671c5368c2541` — add standard C integer-format and API-aware bulk-conversion guidance to `c-type-conventions.md`.
- `adfa8ed7d97da09331c62d597f583a21871856c6` — add contradictory-framing recovery guidance to `tcp-desegmentation-conventions.md`.
