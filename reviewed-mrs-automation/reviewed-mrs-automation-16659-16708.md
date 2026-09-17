# Wireshark MR review automation: !16659-!16708

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest previously unreviewed MRs toward older MRs.

## Exact reviewed MR set

!16708, !16707, !16706, !16705, !16704, !16703, !16702, !16701, !16700, !16699, !16698, !16697, !16696, !16695, !16694, !16693, !16692, !16691, !16690, !16689, !16688, !16687, !16686, !16685, !16684, !16683, !16682, !16681, !16680, !16679, !16678, !16677, !16676, !16675, !16674, !16673, !16672, !16671, !16670, !16669, !16668, !16667, !16666, !16665, !16664, !16663, !16662, !16661, !16660, !16659

Count: **50**.

The historical !17571-!17620 batch remains part of the already-reviewed set and must continue to be counted when selecting later batches.

## Review notes

Merged MRs were weighted more heavily than closed, abandoned, superseded, and release-backport-only MRs. Substantive maintainer discussion was weighted according to reviewer authority and specificity.

- **!16708 (closed/unmerged): Lua compile-warning cleanup.** John Thacker pointed out that `lrexlib` is an externally maintained library and Wireshark should minimize local divergence from its upstream compatibility code, even where some compatibility branches look obsolete in Wireshark's current build matrix. The author agreed and the MR was closed. This is useful negative guidance: vendored/third-party code should not be casually normalized to Wireshark-local assumptions; prefer minimal downstream patches and preserve upstream structure unless there is a compelling project-specific reason.
- **!16707 (merged): generated dissector symbol visibility cleanup.** Martin Mathieson made generated-dissector symbols static where they do not require external linkage. This is consistent with the broader convention of minimizing symbol scope and keeping generated output/tooling aligned.
- **!16700 (merged release-4.0 backport): packetBB TLV parsing.** The parser performs an initial header pass to determine item length and a later pass to add fields; the first pass must consume exactly the same conditional structural bytes (including extended type and two-byte extended length) as the real parse. This reinforces the existing parser-state rule that duplicated/preflight parsing must preserve identical offset semantics.
- **!16690 (merged): documentation CI.** Gerald Combs changed the FAQ artifact to gzip form to avoid CDN email rewriting. Useful operational context but not a general coding convention.
- **!16680 (merged): Export PDUs / Strip Headers Qt dialogs.** Adds consistent application window titles and Help-button integration, with corresponding User's Guide anchors/help-topic wiring. Useful UI consistency example but no new project-wide convention extracted.
- **!16670 (merged): SMB2 notification capability.** Review corrected presentation/naming details around the new notification capability field before merge. This is protocol-specific and does not establish a new durable rule beyond existing field-label consistency guidance.
- **!16660 (merged): IEEE 802.15.4 Enhanced Beacon TLVs.** The contributor supplied a concrete capture and reused the existing Zigbee TLV parser for the payload identified by the specification. The discussion considered heuristic separation to avoid layering concerns. The accepted change is additional evidence for reusing an existing upper-layer parser when the lower-layer specification explicitly identifies that payload, and for supplying a representative capture with protocol extensions.
- **!16659 (merged): Follow UI state.** Wireshark and Logray now explicitly disable Follow actions when no frame is selected rather than leaving action state inherited from the previously selected frame. This reinforces the UI-state principle that context-sensitive actions should be recomputed/cleared when their prerequisite selection disappears.

The remaining MRs in the exact set were reviewed/scanned for state, diff purpose, and substantive discussion; no additional durable coding, architecture, testing, review, or submission rule justified duplicating existing notebook guidance.

## Notebook impact

No separate convention file was modified in this run. The useful findings reinforce existing guidance on third-party/upstream code discipline, parser offset/state consistency, representative captures, reuse of existing dissectors, and context-sensitive UI state. Avoid adding duplicate rules merely to record another corroborating MR.

## Continuation

Rebuild the already-reviewed set from all tracking before the next run. Subject to that rebuild and the same corpus snapshot, the next descending candidate is **!16658**.
