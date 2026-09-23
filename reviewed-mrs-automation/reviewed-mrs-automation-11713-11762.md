# Wireshark MR automation review: !11713–!11762

- **Corpus repository:** `dheitmueller/wireshark-corpus-mrs`
- **Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
- **Notebook starting commit:** `694887ddfba09a389bda1f597b37639bdca610e8`
- **Review direction:** descending MR number, newest available previously-unreviewed first
- **Exact MR count:** 50
- **Disposition:** 50 merged; 0 closed/unmerged

## Selection and duplicate-avoidance

Before selecting this run, all available review tracking in `dheitmueller/wireshark-chatgpt` was reconciled, including `reviewed-mrs.md`, the supplemental automation tracker, and the per-run files under `reviewed-mrs-automation/`. The historical `!17571`–`!17620` batch remains part of the already-reviewed set. The prior mention of `!11762` was only a frontier probe and was not counted as a review.

The exact selected set was:

`!11762, !11761, !11760, !11759, !11758, !11757, !11756, !11755, !11754, !11753, !11752, !11751, !11750, !11749, !11748, !11747, !11746, !11745, !11744, !11743, !11742, !11741, !11740, !11739, !11738, !11737, !11736, !11735, !11734, !11733, !11732, !11731, !11730, !11729, !11728, !11727, !11726, !11725, !11724, !11723, !11722, !11721, !11720, !11719, !11718, !11717, !11716, !11715, !11714, !11713`.

No numeric interval was assumed to be reviewed merely from partial ledger coverage. Each selected MR was checked against the corpus entry; all fifty are merged.

## Durable notebook updates from this run

- `initialization-lifecycle-conventions.md` — Guy Harris's merged master `!11725` establishes declaration-time initialization for process-global configuration defaults when they are compile-time constants, eliminating a hidden initializer-order precondition that TShark/rawshark had failed to satisfy. Commit `10a20ead1f0fe0d3078d42d5982c62288bdc64ef`.
- `c-api-call-contract-conventions.md` — Guy Harris's merged master `!11723` records that `_NOT_SET` values representing absence of a command-line override belong at the configuration boundary, not inside a lower-level setter that would translate “not supplied” into a concrete setting. Commit `9fe752d4d4019526290c66f3ee93bcc4f1cb5b0d`.
- `api-design-conventions.md` — Guy Harris's merged master `!11754` centralizes string-length derivation and 16-bit wire-bound enforcement in `wtap_buffer_append_epdu_string()` instead of making callers manually count bytes; merged `!11753` records the complementary rule to keep a specialized helper internal when external users do not need that entry point, while explicitly considering compatibility of public Thrift extension structures. Guy's `!11722` additionally corroborates semantic rather than mechanism-oriented helper naming. Commit `f7999bb5bc6d426989ad15465f9ba7d88c49cdfd`.
- `assertion-static-analysis-conventions.md` — Martin Mathieson's merged master `!11748` extends `check_typed_item_calls.py` so source-level `value_string` conflicts are checked even when a particular runtime field-registration path would never reach `proto.c`'s equivalent validation. Commit `b3ac43dd5b2fe08c85e3714e9f758a2fd403136b`.

## Per-MR review ledger

| MR | State | Review result |
| --- | --- | --- |
| !11762 | merged | Scheduled generated-data automatic update on master. Routine maintenance; no distinct coding convention extracted. |
| !11761 | merged | Release-4.0 counterpart of the scheduled automatic update. Stable-branch generated-data maintenance only. |
| !11760 | merged | Release-3.6 counterpart of the scheduled automatic update. Stable-branch generated-data maintenance only. |
| !11759 | merged | Manuf-data normalization adds a special case and removes parenthetical text; narrow generated/vendor-name data cleanup. |
| !11758 | merged | Release-note enhancements and fixups; documentation/release maintenance. |
| !11757 | merged | Stig Bjørlykke refactors WSLua `ByteArray` integer accessors toward the TvbRange-style offset/length API, fixes negative-integer handling, and adds tests; useful API-consistency and regression-test evidence. |
| !11756 | merged | ICMPv6 echo-data timestamp heuristic is restricted to the start of the payload, reducing false interpretation of arbitrary echo data; protocol-specific heuristic hardening. |
| !11755 | merged | Removes a now-unused Qt preference after related behavior changed; cleanup of dead configuration surface. |
| !11754 | merged | Guy Harris adds `wtap_buffer_append_epdu_string()`, eliminating caller-supplied manual string lengths and clamping to the 16-bit EPDU TLV limit; promoted to API-design guidance with highest authority. |
| !11753 | merged | Thrift expert-info extension deliberately keeps the specialized expert-aware struct helper internal, avoids unnecessary generalization/removal of older extension state, and was fuzzed before Anders Broman approval; promoted to API-surface guidance. |
| !11752 | merged | John Thacker restores deprecated default-column title aliases such as `_ws.col.Info` for TShark because existing tools/scripts and supported releases depend on the old spelling; strong backward-compatibility evidence without a new notebook section. |
| !11751 | merged | Sharkd avoids a symbol/name collision with MinGW headers; platform portability cleanup. |
| !11750 | merged | Stable-branch backport of the TShark extcap-preference registration fix represented by master `!11743`; corroborating only. |
| !11749 | merged | ICMP timestamp heuristic compares plausible endian interpretations and handles wider `time_t`/clock skew more carefully; protocol-specific heuristic robustness. |
| !11748 | merged | Martin Mathieson extends the typed-item checker to find `value_string` conflicts not covered by runtime field registration; promoted to static-analysis/checker guidance. |
| !11747 | merged | `tshark -G manuf` output includes the short manufacturer name, making machine-readable introspection expose the information consumers need. |
| !11746 | merged | `tshark -G protocols` and `-G heuristic-decodes` append enabled/default/toggle/name metadata, preserving existing leading fields while extending introspection output. |
| !11745 | merged | Fixes a BLF typo introduced by an earlier change; narrow corrective maintenance. |
| !11744 | merged | RPC-with-TLS keeps heuristic dispatch through the AUTH_TLS transition and binds the TLS application handle only after post-upgrade RPC evidence; important protocol-transition architecture evidence, retained in the ledger rather than generalized from a single case. |
| !11743 | merged | TShark registers extcap preferences based on whether the invocation actually needs capture/extcap behavior rather than using an overly broad filename proxy; lifecycle/CLI-condition fix. |
| !11742 | merged | Sharkd `download` lazily creates the requested export-object list if no prior `tap` call populated it, removing a surprising request-order dependency; useful API behavior evidence. |
| !11741 | merged | Packet-list capture-state setup now receives the autoscroll state directly instead of requiring a separate follow-up setter/timer path; Qt state-flow simplification. |
| !11740 | merged | Removes a redundant packet-list tail timer and scrolls when rows are inserted at the existing capture-update cadence; simpler event ownership and smoother UI. |
| !11739 | merged | Home/End navigation preserves horizontal scroll extent consistently with other keyboard movement; focused UI behavior fix. |
| !11738 | merged | BLF AppText/EthernetStatus objects are surfaced through Exported PDU so they appear in the packet list; capture-format integration feature. |
| !11737 | merged | Adds `EXP_PDU_TAG_COL_INFO_TEXT` so Exported-PDU producers can supply Info-column text through the established TLV mechanism; infrastructure feature. |
| !11736 | merged | Updates the generated Sysdig Event dissector and fixes generic syscall Info-column rendering; generated-source and presentation maintenance. |
| !11735 | merged | Martin Mathieson fixes display filters, field widths, and related metadata; corroborates the notebook's existing field-definition correctness guidance. |
| !11734 | merged | IOConsoleDialog is destroyed on close rather than merely hidden/left alive; corroborates existing Qt object-lifetime guidance. |
| !11733 | merged | CMake accepts any supported Lua version by default rather than preferring an unnecessarily narrow version; dependency-discovery/build flexibility change. |
| !11732 | merged | ManufDialog persists/restores geometry using the standard dialog geometry mechanism; UI consistency maintenance. |
| !11731 | merged | Lua console switches to a vertical splitter, uses an appropriate text widget, and uses the proper macOS shortcut; focused Qt usability work. |
| !11730 | merged | Capture-options tab ordering is made sequential and the MR includes explicit keyboard-navigation testing; practical UI accessibility/regression evidence. |
| !11729 | merged | CredentialsDialog persists/restores geometry; UI consistency maintenance. |
| !11728 | merged | BATADV avoids `proto_tree_add_bitmask()` when there are no constituent bit fields, because the helper requires a non-empty field set; API-precondition correctness evidence. |
| !11727 | merged | Manufacturer-data wording cleanup; generated/data presentation maintenance. |
| !11726 | merged | Telnet unescaping stops mixing offset domains and directly tests whether the required TVBuff offset exists; validated against two captures plus 7,500 fuzz iterations, reinforcing bounds-driven parsing/testing practice. |
| !11725 | merged | Guy Harris replaces ordering-sensitive `dissect_opts_init()` with declaration-time designated initialization so all frontends receive sentinel defaults; promoted to initialization-lifecycle guidance. |
| !11724 | merged | StockIconToolButton explicitly uses the correct cursor and restores normal icon mode on leave; narrow Qt event-state fix. |
| !11723 | merged | Guy Harris moves `TS_*_NOT_SET` handling to TShark/rawshark callers so timestamp setters no longer conflate “no override” with a concrete setting; promoted to API contract guidance. |
| !11722 | merged | Guy Harris renames `slist_break_commas()` to `process_enable_disable_list()` because the semantic operation is applying an enable/disable list, not merely splitting strings; recorded as corroboration in API naming guidance. |
| !11721 | merged | Editcap gains `--discard-packet-comments` and fixes `-a` to actually replace existing comments as documented; command behavior is brought into alignment with its user-facing contract. |
| !11720 | merged | Sharkd enables synchronous name resolution and stops an inherited MaxMind helper via the UAT reset callback rather than `uat_clear()`, preserving configured database paths while resetting process state; useful lifecycle/configuration evidence. |
| !11719 | merged | Adds MBIM Microsoft Voice Extensions NITZ dissection; protocol feature work. |
| !11718 | merged | Documents that `-t` timestamp settings update recent-file state and therefore persist into later GUI invocations; important user-visible configuration semantics, documentation-focused. |
| !11717 | merged | Sharkd `tap` gains an optional display-filter parameter propagated to tap listener registration; API feature with a straightforward contract. |
| !11716 | merged | Miscellaneous dissector masks, item lengths, and label/filter consistency fixes; corroborates field metadata correctness rules. |
| !11715 | merged | DHCP failover Microsoft extensions are implemented from published documentation and accompanied by representative capture files; good new-protocol/feature submission evidence. |
| !11714 | merged | STUN binding requests expose Change IP/Port semantics in the Info column; presentation feature, no cross-cutting rule extracted. |
| !11713 | merged | Adds the MDB dissector with a public protocol specification and documented pcap input format; while an official DLT was pending, User-DLT provided an explicit testing path. Maintainer review was incorporated before Anders Broman merged it. |

All fifty selected MRs were merged, so no abandoned or superseded proposal required reduced weighting in this batch. The highest-weight evidence came from merged, maintainer-authored changes by Guy Harris (`!11754`, `!11725`, `!11723`, `!11722`), followed by substantive accepted work from Martin Mathieson, John Thacker, Stig Bjørlykke, Anders Broman, and other maintainers/contributors. Discussions, diffs, tests, and follow-up/backport relationships were considered where they materially affected the durable conclusion rather than treating MR titles alone as authoritative.

## Frontier probe (not reviewed)

`!11712` exists in the corpus, is merged, and is titled `sharkd: initialize columns correctly for sip stats`. It was inspected only to confirm that the corpus continues below this batch and is **not** part of the reviewed set recorded above.
