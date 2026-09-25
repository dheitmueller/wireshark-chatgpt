# Review findings: !9562–!9612 (mr_9604 absent)

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed corpus MRs. Selection was reconstructed from the notebook's explicit review tracking rather than inferred range coverage. `mr_9604.json` is absent from this corpus snapshot, so the exact set spans !9612 down through !9562 with that one numeric gap. The historical !17571–!17620 batch remains preserved and counted as 50 reviewed MRs.

Outcome weighting: 48 selected MRs are merged. !9603 and !9600 are closed/unmerged and were down-weighted. !9610 did merge, but it was quickly reverted by previously reviewed !9647 after macOS runtime breakage, so its implementation is treated as negative/diagnostic evidence rather than a preferred design.

## Durable findings promoted

### Static source checking must not be bypassed by ordinary formatting

Merged !9592 fixes TWAMP MBZ field registrations from `FT_UINT8` to `FT_UINT16`. Martin Mathieson explicitly identified why the existing checker missed the bug: it did not accept whitespace between the function name and opening parenthesis. Martin's merged !9593 fixes the regular expressions. This is now recorded in `source-checker-conventions.md`: legal formatting variation must not determine whether a semantic check runs.

### Typed-item checking should validate semantic argument domains

Merged !9562, authored and merged by Martin Mathieson, extends `check_typed_item_calls.py` so `proto_tree_add_item()` / `ptvcursor_add()` encoding parameters are checked rather than accepted merely because they are integer-compatible. Running it exposed real cases where decoded packet values or arbitrary numeric values had been supplied as the final encoding argument. The fixes use explicit `ENC_BIG_ENDIAN`, `ENC_NA`, or other appropriate flags. Promoted to both `source-checker-conventions.md` and `field-decoding-api-conventions.md`.

### Zero/nonzero protocol semantics should use Boolean fields and Boolean-return helpers

In merged !9597, Pascal Quantin points out that Exported-PDU's reassembly flag is specified as nonzero when true. The accepted implementation uses `FT_BOOLEAN` plus `proto_tree_add_item_ret_boolean()` rather than relying on a 0/1 integer convention. Pascal also catches the output-pointer contract of `proto_tree_add_item_ret_uint()`; the accepted code receives into `guint32` before narrowing into smaller state members. Promoted to `field-value-semantics-conventions.md`.

### Capacity, initialized bytes and logical buffer length are separate states

Merged !9596 exposes a pre-existing Wiretap buffer contract while introducing Exported-PDU construction helpers. Appending raw packet bytes at the start of a nonempty buffer would overwrite already-built metadata. The accepted path writes at `ws_buffer_end_ptr()`, grows logical length only after a successful read, and explicitly cleans buffers at top-level record-read boundaries. Promoted to `c-api-call-contract-conventions.md`.

### pcapng metadata discovery is streaming, both before and between packets

John Thacker's merged !9573 makes `pcapng_open()` process all initially encountered built-in internal blocks, including NRBs/DSBs before an IDB, but conservatively avoids guessing about plugin/custom block types. His merged !9608 then preserves NRBs discovered later by exposing a growing reader-owned array to dumpers and tracking emitted/seen entries. Together they show that file-level metadata cannot be modeled solely as a fixed open-time header. Promoted to `capture-metadata-discovery-conventions.md`.

## Strong corroborating and negative evidence

- !9610 is useful precisely because its accepted Linux relocation change failed on macOS: the test run exposed executables linked against both static and dynamic wsutil, Gerald Combs initiated the !9647 revert, and João Valverde asked for native macOS MR coverage. This corroborates the existing rule that compatibility-sensitive build/link changes need CI jobs that actually exercise the affected platform. The implementation itself is not promoted because it was reverted.
- !9607 provides additional filesystem-layout evidence. Guy Harris distinguishes macOS bundles and Windows relative layouts from traditional Unix install trees; Gerald Combs calls out AppImage as another relocatable deployment. The accepted work is later refined by the already-reviewed !9621 packaging-mode rule, so no duplicate convention was added.
- !9569 is good review-process evidence for packaging work: John Thacker explicitly offers to use mock builds across distributions so the fix is not developed through blind attempts, then follows with merged !9574. Existing CI/platform validation guidance already covers the broader lesson.
- !9583 and !9581 show how new/large dissectors are reviewed as systems rather than just feature diffs: project checkers, duplicate filter identifiers/types, dead stores, field-width errors, naming, helper selection and spelling all matter before merge.
- !9582 supplies another representative-capture example for protocol work; existing sample-capture guidance is already stronger and more recent.
- !9600 is closed but gives a clear workflow rule: direct translation-file edits are redirected to Transifex. Later notebook evidence already records this, so the closed MR is retained as corroboration only.
- !9603 was closed without merge and references not-yet-available external documentation; it contributes little accepted architectural weight.

## Frontier

The corpus is not exhausted. `mr_9561.json` exists at the same corpus commit; it is merged and titled `AUTHORS: Fix non-standard entry format`. It was inspected only as a frontier probe and is not counted as reviewed in this run.
