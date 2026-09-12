# Wireshark MR review batch !22743-!22792

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base before this run: `79fff346192311543c71a5c3ac2b1fbcd152a9d5`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the aggregate `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all available per-run files under `reviewed-mrs-automation/`; then selected the fifty highest-numbered corpus MRs not in that set. The historical !17571-!17620 batch remains reviewed and counted. This run reviews exactly 50 MRs: !22792 through !22743 inclusive.

| MR | Status | Review notes |
|---|---|---|
| !22792 | Deep | Merged, John Thacker-authored Netscaler writer timestamp fix. Corrects absolute-time field/width and big-endian copy behavior; also documents that sequential reads and seek reads do not have identical timestamp context for old Netscaler formats. Corroborates existing wiretap random-access-state guidance. |
| !22791 | Scanned | Merged John Thacker Coverity cleanup: check and log otherwise-unexpected libgcrypt failures in KNX/IP. Reinforces checking fallible library calls. |
| !22790 | Scanned | Merged John Thacker TTL ownership cleanup; recognizes that `xmlXPathCastToString` allocates and removes/leaves no redundant ownership path. Existing lifetime guidance covers it. |
| !22789 | Scanned | Merged John Thacker WSLua defensive check for `lua_get_metatable` failure. No new convention beyond consistent failure handling. |
| !22788 | Scanned (closed draft) | Draft test update for Lua globals was closed unmerged. Down-weighted; no accepted implementation lesson. |
| !22787 | Scanned | Merged John Thacker zlib cleanup checks `inflateInit2` failure consistently with other call sites. Existing error-return guidance applies. |
| !22786 | Deep | Merged John Thacker change uses `ws_abort_if_fail` for an invariant that must hold even when ordinary assertions are compiled differently, allowing static analysis to understand `mark_truncated` bounds. Strong corroboration of existing assertion/static-analysis guidance. |
| !22785 | Scanned | Merged generated-dissector-list comment cleanup explicitly tells developers to run `generate-dissector-lists.py`. Reinforces generated-artifact workflow; no separate rule needed. |
| !22784 | Scanned | Merged Martin Mathieson checking-script refactor standardizes future results on a Result type. Tooling consistency improvement without broader review lesson. |
| !22783 | Scanned (open snapshot) | Gerald Combs revert of Stratoshark project-version inheritance remained open in the corpus snapshot. Down-weighted as provisional build/version policy. |
| !22782 | Scanned | Merged NATS header-field enhancement exposes semantic version components and header name/value fields and shortens filter paths. Useful field modeling, but no substantive human design discussion in the snapshot. |
| !22781 | Scanned | Merged User's Guide typo correction only. |
| !22780 | Scanned | Merged John Thacker Coverity-oriented wireless-timeline cleanup makes an indexing invariant clearer to static analysis. Corroborates existing analyzer-model guidance. |
| !22779 | Scanned | Merged pcapng Darwin custom-block cleanup frees and clears error state when intentionally ignoring an unknown-option error. Existing ownership/error-state guidance covers it. |
| !22778 | Scanned | Merged PEAK TRC leak cleanup; no new durable convention. |
| !22777 | Scanned | Merged release-4.4 backport correcting a stale `-1` return after a parser return type became boolean. Same master fix as !22773; no additional lesson. |
| !22776 | Scanned | Merged release-4.6 backport of the Netscreen boolean-return fix represented by !22773. |
| !22775 | Scanned | Merged procmon leak cleanup from Coverity; existing lifetime guidance applies. |
| !22774 | Scanned | Merged John Thacker nettl open-path cleanup delays private-data allocation until the file has actually been accepted, reducing NOT_MINE/error-path ownership. Good wiretap opener hygiene; covered by existing lifetime/resource rules. |
| !22773 | Scanned | Merged master Netscreen fix replaces the leftover `-1` failure return after conversion from int to boolean. Reinforces auditing all return sites when changing API result domains. |
| !22772 | Scanned | Merged Qt defensive `dynamic_cast` failure check. Coverity-driven, no new architecture lesson. |
| !22771 | Deep | Merged John Thacker change stops passing `-1` to `tvb_get_ptr` for a single field. On composite TVBuffs that sentinel can force an expensive copy of all remaining bytes; explicit captured length permits a bounded copy or direct pointer. Corroborates explicit unsigned/bounded TVBuff API direction. |
| !22770 | Scanned | release-4.6 backport increasing macOS `aqtinstall` network timeout; CI reliability maintenance only. |
| !22769 | Scanned | Automated master data/translation update. No reusable engineering lesson. |
| !22768 | Scanned | Automated release-4.6 data/translation update. No reusable lesson. |
| !22767 | Scanned | Automated release-4.4 data/translation update. No reusable lesson. |
| !22766 | Scanned | Merged declaration of the NATS protocol header as public following the project announcement. Fits existing public-header/API guidance. |
| !22765 | Scanned | Merged const-correctness cleanup moves read-only EPAN tables to read-only storage. Good hygiene but no Wireshark-specific convention. |
| !22764 | Deep | Merged John Thacker API cleanup changes `tvb_memcpy`/`tvb_memdup` offset and length domains to unsigned, noting no repository users require negative sentinels and that callers wanting the remainder can obtain captured remaining length explicitly. Strong corroboration of the notebook's signed-sentinel-to-explicit-API migration guidance. |
| !22763 | Scanned | Merged ASTERIX const cleanup plus UBSan fix for signed `1 << 31`, and use of `array_length`. Reinforces existing C arithmetic/UB guidance. |
| !22762 | Scanned | Merged RTPS cleanup uses `tvb_memdup` directly for packet-scope-owned storage instead of `tvb_get_ptr` plus duplicate/free bookkeeping. Existing packet-memory ownership guidance covers it. |
| !22761 | Scanned | Merged WiMax const-correctness cleanup found while auditing casts away from `tvb_get_ptr`'s const result. No new rule. |
| !22760 | Discussion-focused | Merged TAC name-resolution feature with sample config and GTPv2/S1AP captures. Anders Broman approved it and later suggested possible pcapng name-resolution-block integration; post-merge discussion shows that extension remains optional/provisional, so it was not promoted into architecture guidance. |
| !22759 | Scanned | Merged John Thacker TLS utility signature correction makes an input pointer const to match libgcrypt and eliminates casts away from `tvb_get_ptr` constness. Reinforces const-correct API boundaries. |
| !22758 | Deep | Merged NATS operation-tree redesign. John Thacker rejected hidden filter-only fields and proposed visible `FT_NONE` operation items/subtrees under one NATS protocol item; author reworked the diff, supplied a sample capture and before/after tree screenshots, fixed a payload-length issue, and John approved. Promoted to `protocol-field-semantic-conventions.md`. |
| !22757 | Scanned | Merged NATS support for +OK/-ERR operations; straightforward protocol completeness change. |
| !22756 | Deep | Merged John Thacker TVBuff alignment fix permits zero-length output while rejecting invalid negative counts. Later RHEL8/GCC feedback exposed signed-domain analyzer trouble; John explicitly noted moving away from signed lengths is the right direction. Corroborates existing unsigned-domain and invariant guidance. |
| !22755 | Scanned | Master macOS `aqtinstall` timeout increase; CI reliability maintenance, with !22770 as backport. |
| !22754 | Deep | Merged John Thacker documentation/test cleanup removes unsupported `tvb_memdup(..., -1)` semantics; no repository callers used the sentinel. Together with !22764/!22771 this reinforces explicit bounded lengths instead of overloaded negative sentinels. |
| !22753 | Scanned | Documentation-only removal of a reference to a file whose content moved into the WDSG. |
| !22752 | Deep | Merged public-header compile validation with 36 discussion notes. John Thacker challenged a redundant include and asked whether dependency generation should already supply it; the author removed it after fixing headers. Strong early evidence for self-contained/public-header testing, already superseded in the notebook by the later !22933 rule. |
| !22751 | Scanned | Generated ASTERIX spec sync fixing typos; no reusable review lesson. |
| !22750 | Discussion-focused | Merged Lua 5.0 globals baseline/test regeneration work. Useful release-test maintenance; no new coding convention promoted. |
| !22749 | Scanned | Merged minor leak fixes from Coverity. Existing lifetime guidance applies. |
| !22748 | Scanned | release-4.6 backport of RTP audio `QAudioSink::reset` behavior fix represented by !22743. |
| !22747 | Scanned | Merged removal of obsolete comment about `tvb_memdup(..., -1)` semantics; part of the same explicit-length cleanup represented by !22754/!22764. |
| !22746 | Scanned | Dependency update to c-ares 1.34.6. No reusable engineering lesson. |
| !22745 | Scanned | Merged InfiniBand/SMB Direct initial-negotiate fix with a concrete capture and frame-by-frame reassembly validation. Strong corroboration of capture-based protocol validation, already well established. |
| !22744 | Discussion-focused | Merged first public P4RPC dissector after extensive review (43 notes), including Martin Mathieson cleanup feedback and iterative revisions. New protocol supports TCP and TLS decryption. The review mainly reinforces existing scope/static/helper/style expectations; no distinct new convention promoted from this run. |
| !22743 | Scanned | Merged John Thacker RTP audio fix uses `QAudioSink::reset()` rather than `stop()` when immediate buffer discard is required, based on documented Qt semantics and observed Windows behavior; also initializes members flagged by Coverity. Platform/UI-specific. |

## Durable notebook change from this batch

- Added a protocol-field semantic rule from !22758: prefer a meaningful visible tree item/subtree over a hidden field created only to make a concept filterable. If presence of an operation/message is itself the semantic value, an `FT_NONE` item can model it directly and provide the filter naturally.

## Strong corroboration retained without duplicate rules

- !22752 reinforces public-header self-containment/compile validation already captured more strongly from later !22933.
- !22754, !22756, !22764, and !22771 reinforce the migration away from signed lengths and `-1` sentinels toward explicit bounded/remaining-length TVBuff APIs.
- !22745 reinforces supplying a concrete capture and validating affected and unaffected reassembly behavior.
- !22786 reinforces using an unconditional invariant mechanism when code correctness and analyzer understanding must not depend on debug-assert configuration.

No more than these 50 MRs were reviewed in this run.