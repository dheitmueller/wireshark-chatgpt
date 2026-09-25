# Review findings: Wireshark MRs !9512–!9561

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs, !9561 through !9512. Forty-nine are merged; !9526 is closed/unmerged. !9512 is recorded as negative evidence because merged !9516 reverted it within hours after review exposed a broken field registration.

## Durable findings

### Capture metadata must survive and be replayable across redissection

Merged master !9550, authored by John Thacker, stores pcapng Name Resolution Block mappings as durable Wiretap block data and replays already-read NRBs when fresh IPv4/IPv6 callbacks are installed. This fixes redissection after higher-level state is torn down without physically rereading every non-packet block.

**Rule:** metadata that rebuilds dissection state must be retained by the capture reader and replayed when a new consumer attaches. This sharpens the existing !9573/!9608 rule that pcapng metadata is a stream rather than only an open-time header.

### CI must build the exact source revision that caused the job

Merged !9545 fixes MSYS2 CI so the package recipe checks out the workflow commit rather than the moving tip of master.

**Rule:** if a CI packaging layer performs its own source checkout, propagate the immutable commit identity into the package recipe. A job for commit A must not silently compile later commit B.

### Reset analysis state at protocol lifecycle boundaries

Merged !9554 resets USBLL endpoint reassembly/retransmission state when SET ADDRESS establishes a new nonzero device address. It deliberately preserves default-address bootstrap state needed for control enumeration.

**Rule:** reassembly, retransmission, and session state follow protocol identity/lifecycle. Clear state when an authoritative protocol event creates a new logical identity, while preserving only narrowly defined facts that remain valid by protocol semantics.

### Put reusable custom presentation in field metadata

Merged !9527, authored by John Thacker, moves DHCP time formatting from one-off tree-item formatting into registered `BASE_CUSTOM` formatter metadata. The formatted representation then also appears in custom columns and other consumers.

**Rule:** when custom textual presentation is part of the field's general display contract, register it with the field rather than formatting only one protocol-tree insertion.

### Grouping-only tree items should use no-value field semantics

Merged !9512 added an O-RAN grouping field as an always-empty `FT_STRING` with a nonzero mask and was merged before a pipeline passed. John Thacker said string/no-value fields cannot carry that integer mask and that `FT_NONE` was the appropriate type for an item used only as a grouping node. Lars Völker reported breakage. Merged !9516 promptly reverted the change; Martin Mathieson noted that the revert unbroke the build.

**Rule:** synthetic grouping nodes should use no-value semantics such as `FT_NONE`, not a fake empty string, and masks must match the registered field type. Applicable CI/checker coverage should pass before merge; a prematurely merged and immediately reverted change is negative evidence, not an accepted exemplar.

### Set semantics need a scalable set representation

Merged !9538 deduplicated dependent-frame entries with linear list membership and noted the scalability concern in its source. Tomasz Moń later reported a real capture becoming effectively unusable; already-reviewed merged !9622 replaced the representation with a hash table and eliminated the high CPU usage.

**Rule:** when the semantic object is a potentially large unique membership set, use a set/hash structure rather than repeated linear scans.

## Corroborating evidence

- !9552 fixes many real field-width, item-length, duplicate-filter and mask defects found by `check_typed_item_calls.py`; its false-positive handling remains narrow, reinforcing existing checker guidance.
- !9559 includes Gerald Combs's request for release notes for the personal extcap-path move, corroborating release-note expectations for user-visible changes.
- !9542 includes Gilbert Ramirez catching use of a generated item outside the branch that creates it.
- !9530 includes a requested sample capture and later John Thacker protocol-semantic questioning of the generic non-NTLM dispatch; retain that as post-merge review evidence rather than a broad dispatch precedent.
- !9520 had a post-merge sibling-field label correction, later handled by !9588.
- !9519/!9551 align the H.261 one-byte field with `ENC_NA`, corroborating encoding-argument guidance.
- !9526 was closed as superseded by later functionality and is down-weighted.

## Frontier

`mr_9511.json` exists at the same corpus commit, is merged, and is titled `RPM: Fix header installation`. It was inspected only as the next-frontier probe and is not counted as reviewed.
