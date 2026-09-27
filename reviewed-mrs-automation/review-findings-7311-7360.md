# Review findings: !7311-!7360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed MRs were examined, !7360 down through !7311. The exact set and selection proof are in `reviewed-mrs-automation-7311-7360.md`. Outcome mix: **49 merged, 1 closed/unmerged (!7351)**. Merged work is primary evidence; !7351 is down-weighted.

## High-value findings

### !7348 — return-value APIs cannot depend on protocol-tree construction

Merged !7348 added `proto_tree_add_bitmask_list_ret_uint64()` but only extracted and assigned the returned value inside `if (tree)`. Guy Harris later stated explicitly that this is invalid: every `proto_tree_add..._ret...` helper must return its semantic value even when no protocol tree is being built. He tied the defect to bug #18203 and pointed to merged !7432 as the fix.

Treat !7348 as negative/superseded implementation evidence despite its merged status; Guy's later diagnosis and !7432 are authoritative. Tree construction is optional presentation, while a `_ret_` helper's semantic output is part of the API contract.

### !7329 — avoid nested Qt event loops; make asynchronous menu lifetime explicit

Tomasz Moń replaced widespread `QMenu::exec()` calls with `popup()`. In review he clarified that `exec()` creates a nested `QEventLoop` in the same GUI thread; it does not create a worker thread. That re-entry can expose state to callbacks at points callers do not expect. Roland Knall raised the companion lifetime issue: changing from synchronous stack-lifetime menus to asynchronous popup behavior changes object lifetime. The accepted implementation moves ephemeral menus to explicitly owned heap objects and uses delete-on-close where needed.

### !7333, !7336, !7337 — heuristic recognition should accumulate protocol evidence before claiming traffic

John Thacker's Diameter and Apache Tribes work provides a compact heuristic series. !7333 adds Diameter-over-TCP heuristic dispatch, disabled by default, and binds the conversation to the normal Diameter TCP dissector only after recognition. !7337 strengthens Diameter recognition by requiring a plausible minimum message length and 32-bit alignment. !7336 avoids trusting Apache Tribes' unregistered port and instead uses the fixed ASCII `TRIBES-B` delimiter as a content signature.

### !7330 / !7357 and !7331 / !7356 — buffer lengths have domains

Master !7330 and release-3.6 backport !7357 fix `protoo_strlcpy()`. `g_strlcpy()` reports source length, but this wrapper's callers use its return value as the number of bytes actually materialized and as a later append offset. On truncation, returning `dest_size` could move the next offset one byte past the buffer. The accepted logic handles zero capacity and otherwise caps the result at `dest_size - 1`.

Master !7331 and backport !7356 fix several built-in `addr_to_str` implementations that performed fixed-width writes without honoring the supplied `buf_len`.

### !7358 — semantic configuration data must not inherit UI presentation limits

John Thacker removes `COL_MAX_LEN` from custom-column expression parsing. Expression length has no necessary relationship to displayed column width, especially when a column expression combines many alternative protocol fields. The parser switches from a fixed temporary buffer to `GString`.

### !7315 (plus !7316 / !7317) — model the actual bitfield container

Guy Harris's master !7315, with release-3.6 and release-3.4 backports !7316/!7317, treats the IEC 104 four-octet control field as one little-endian `FT_UINT32` container and defines masks for the semantic subfields. I frames and S/U frames get distinct type masks because their type encodings are genuinely different. `proto_tree_add_item_ret_uint()` obtains decoded Tx/Rx/U-type values from the same field definitions displayed to users.

### !7312 — static-analysis findings are part of review for large dissector additions

Initial Wi-Fi 7/EHT support drew an Alexis La Goutte review request to fix multiple Clang Analyzer dead-store warnings in the new radiotap/802.11 code. The accepted commit series includes explicit analyzer cleanup.

## Additional reviewed evidence

!7359 aligns CLI configuration-profile semantics with the GUI by copying a global-only profile into the personal profile location before use. !7321 includes a focused DTLS Connection ID block-cipher reference capture from Stig Bjørlykke, corroborating representative-capture practice. !7344 adds regression tests for display-filter slice existence checks. The remaining MRs in this batch were reviewed but were maintenance, spec updates, generated-data updates, small UI/build fixes, refactors, or protocol-specific changes that did not justify new durable notebook rules.
