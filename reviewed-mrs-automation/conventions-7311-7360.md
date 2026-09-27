# Conventions extracted from !7311-!7360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Tree-independent semantic return values

If an API is named and documented as a `proto_tree_add..._ret...` helper, the returned semantic value is part of the API contract and must be produced regardless of whether a protocol tree is being built. Tree construction is optional presentation state.

Merged !7348 violated this by assigning its return value only inside `if (tree)`. Guy Harris later diagnosed the defect explicitly, tied it to bug #18203, and pointed to merged !7432 as the correction. Treat !7348 as negative/superseded evidence and !7432 as the accepted contract.

**Rule:** extract/compute the semantic value first; gate only tree mutation on `tree != NULL`.

## Avoid unnecessary nested Qt event loops

Merged !7329 replaces `QMenu::exec()` with `popup()` across the Qt UI. Tomasz Moń clarified during review that `exec()` creates a nested `QEventLoop` in the same GUI thread, which permits re-entry at otherwise unexpected points. It is not a separate worker thread.

**Rule:** when synchronous blocking semantics are not required, stay on the ordinary application event loop rather than nesting another event loop.

**Lifetime rule:** replacing synchronous stack-lifetime UI objects with asynchronous popup behavior changes lifetime requirements. Give ephemeral objects explicit parents/ownership and destruction policy; do not merely swap `exec()` for `popup()`.

## Heuristics: recognize first, then claim

Merged !7333 adds a disabled-by-default Diameter-over-TCP heuristic. After positive recognition, it binds the conversation to the normal Diameter TCP dissector and uses ordinary TCP PDU handling. Merged !7337 strengthens the recognizer with the protocol's 32-bit alignment invariant. Merged !7336 identifies Apache Tribes traffic by its fixed ASCII `TRIBES-B` delimiter instead of trusting an unregistered port.

**Rule:** build heuristic confidence from strong signatures and independent structural invariants. Return a clean negative result before mutating packet presentation or conversation ownership. Bind a TCP conversation to the regular dissector only after sufficient recognition succeeds.

## Distinguish string-length domains

Merged !7330 (and backport !7357) fixes a wrapper whose callers need the number of bytes actually copied, while the underlying `g_strlcpy()` reports the source length. On truncation, an append offset must reflect materialized bytes, not required/source length.

Merged !7331 (and backport !7356) fixes fixed-format address renderers that ignored the caller's `buf_len`.

**Rule:** distinguish source/logical length, required capacity, destination capacity, and bytes actually written. A value used for subsequent destination pointer arithmetic must be in the materialized-byte domain. Even fixed-width formatting routines must honor caller capacity.

## Semantic configuration strings must not inherit presentation limits

Merged !7358 removes `COL_MAX_LEN` from custom-column expression parsing and replaces a fixed parser buffer with `GString`. Expression length and displayed column width are different domains.

**Rule:** do not apply a UI presentation bound to persisted semantic configuration unless the configuration format itself defines that limit.

## Model multi-byte control words as their real bitfield container

Guy Harris's merged !7315, with !7316/!7317 backports, treats the IEC 104 four-byte control field as one little-endian 32-bit value and registers masks for its semantic subfields. Different frame forms use different type masks, and `proto_tree_add_item_ret_uint()` provides decoded values from the same registered fields used for display.

**Rule:** when the wire format defines one logical control word split into subfields, register the correct containing integer/endianness and masks rather than separately reconstructing disconnected fragments. Prefer add-and-return field APIs when program logic needs the same decoded value shown in the tree.

## Static-analysis hygiene on substantial dissector additions

In merged !7312, Alexis La Goutte requested that multiple Clang Analyzer dead-store warnings in the new Wi-Fi 7/radiotap code be fixed; the accepted commit series includes those fixes.

**Rule:** run applicable static-analysis/checker tooling over substantial new dissector code and resolve actionable warnings before submission/merge.

## Frontend behavior should stay semantically consistent

Merged !7359 makes command-line profile selection follow the GUI behavior when a named profile exists only globally: copy it into the personal profile area and then use it.

**Rule:** when GUI and CLI expose the same user concept, avoid silently divergent semantics unless the difference is intentional and documented.
