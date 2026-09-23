# Wireshark MR automation review: !11763–!11812

- **Corpus repository:** `dheitmueller/wireshark-corpus-mrs`
- **Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
- **Review direction:** descending MR number, newest available previously-unreviewed first
- **Exact MR count:** 50
- **Disposition:** 50 merged; 0 closed/unmerged

## Selection and duplicate-avoidance

Before selecting this run, all available review tracking in `dheitmueller/wireshark-chatgpt` was reconciled, including `reviewed-mrs.md` and the per-run files under `reviewed-mrs-automation/`. The historical `!17571`–`!17620` batch remains part of the already-reviewed set. The prior mention of `!11812` was only a frontier probe and was not counted as a review.

The exact selected set was:

`!11812, !11811, !11810, !11809, !11808, !11807, !11806, !11805, !11804, !11803, !11802, !11801, !11800, !11799, !11798, !11797, !11796, !11795, !11794, !11793, !11792, !11791, !11790, !11789, !11788, !11787, !11786, !11785, !11784, !11783, !11782, !11781, !11780, !11779, !11778, !11777, !11776, !11775, !11774, !11773, !11772, !11771, !11770, !11769, !11768, !11767, !11766, !11765, !11764, !11763`.

No numeric interval was assumed to be reviewed merely from partial ledger coverage.

## Durable notebook updates from this run

- `tvbuff-parsing-conventions.md` — high-authority merged master MR `!11805`, authored and merged by Guy Harris, establishes progressive TVBuff reads so truncated packets are dissected as far as safely possible instead of aborting early because presentation code prefetched later fields. Stable backport `!11776` independently reinforces keeping delimiter searches and packet-derived text parsing within TVBuff bounds rather than raw C-string pointer walking. Commit `4d5cacce2f9eb445cc4602487bb7fe89776e54b1`.
- `c-portability-conventions.md` — merged master MR `!11809`, authored and merged by João Valverde, establishes that callbacks such as `bsearch()` comparators must match the concrete element type/layout actually passed by the library rather than reinterpreting an unrelated struct with coincidentally similar leading members; the old assumption caused unaligned access. Commit `c67cf9eddcbfb4f003230b46d98b4de4bc51e77d`.
- `resource-lifetime-scope-conventions.md` — merged master MR `!11768`, authored and merged by João Valverde, records that a GUI/process object surviving Lua reload must not retain pointers into the destroyed `lua_State`; runtime-dependent pointers/callback state must be reacquired or re-registered for the new runtime generation. Commit `18c9de54ea8643e9aa6e00002e502f655c4aeaa7`.

## Per-MR review ledger

| MR | State | Review result |
| --- | --- | --- |
| !11812 | merged | Adds RDSTLS packet parsing to RDP. Review caught checker/static-analysis issues, reinforcing that new dissector paths should be run through Wireshark's field/checker tooling as well as normal compilation. |
| !11811 | merged | Release-3.6 backport of Kafka SyncGroup `instance_id` version gating; protocol field is decoded only for the API versions that actually carry it. |
| !11810 | merged | Release-4.0 backport of the same Kafka SyncGroup version-gating fix; corroborates the merged master behavior. |
| !11809 | merged | João Valverde fixes an unaligned access in manuf lookup by using a comparator matching the actual searched-array element type; promoted to C portability guidance. |
| !11808 | merged | John Thacker addresses Clang Analyzer warnings/dead stores; useful static-analysis hygiene, no separate rule beyond existing checker guidance. |
| !11807 | merged | BLF Ethernet status objects expose interface name and hardware channel metadata; protocol/capture-format feature work, scanned. |
| !11806 | merged | Release-note maintenance; no distinct engineering convention. |
| !11805 | merged | Guy Harris changes PGM to fetch fields only when reached so truncated packets still display all safely available earlier fields; promoted with highest authority in this run. |
| !11804 | merged | Release-note maintenance; scanned. |
| !11803 | merged | Release-3.6 version bookkeeping; no durable convention. |
| !11802 | merged | Release-4.0 version bookkeeping; no durable convention. |
| !11801 | merged | Browser SSL-keylog dialog usability update (launch/reset behavior); narrow UI improvement. |
| !11800 | merged | Build/release preparation for 3.6.16; release bookkeeping. |
| !11799 | merged | Build/release preparation for 4.0.8; release bookkeeping. |
| !11798 | merged | BLF named-flags mask correction; field metadata correctness evidence, no new cross-cutting rule. |
| !11797 | merged | Decode As Qt model removes and deletes the owned item rather than merely dropping the pointer from the container; corroborates existing Qt ownership/lifetime guidance. |
| !11796 | merged | Release-3.6 backport of the reverse-IPv6 lookup pointer-level fix; corroborates master `!11791`. |
| !11795 | merged | Release-4.0 backport of the reverse-IPv6 lookup pointer-level fix; corroborates master `!11791`. |
| !11794 | merged | Adds DNS URI record dissection; review requested a representative capture and suggested a `proto_tree_add_item_ret_uint()` path to avoid duplicate fetching. |
| !11793 | merged | Camel TimeAndTimezone display correction; narrow protocol-field fix. |
| !11792 | merged | IEEE 802.11 EHT Action-frame support with iterative build/review fixes; substantial protocol feature work, no stronger generic rule extracted. |
| !11791 | merged | Peter Wu fixes IPv6 external reverse lookup by passing the actual address bytes rather than a pointer-to-pointer; direct API pointer-contract correctness fix. |
| !11790 | merged | USBLL control-transfer reassembly accounts for setup bytes when computing host-data length; protocol/reassembly-specific correction. |
| !11789 | merged | Lua initialization loading moves to the plugin directory while retaining old config-location compatibility with a warning; useful migration/backward-compatibility evidence. |
| !11788 | merged | Release-note/security-advisory maintenance; scanned. |
| !11787 | merged | Removes the Lua `package.prepend_path` helper as unnecessary/hacky; cleanup, no general convention promoted. |
| !11786 | merged | Master Kafka SyncGroup `instance_id` parsing is explicitly version-gated; accepted behavior preserved by `!11810`/`!11811`. |
| !11785 | merged | Martin Mathieson expands value-string checker coverage for duplicate labels and suspicious single gaps; reinforces treating field/value metadata checks as correctness tooling. |
| !11784 | merged | Restores `ipmap.html` to the runtime data-file installation location rather than documentation location; packaging/runtime-data correction. |
| !11783 | merged | Fixes sharkd `voip-calls.stop_time` copy/paste bug and updates the unit test, reinforcing output-contract regression coverage. |
| !11782 | merged | Moves stable/deprecated Lua initializations and compatibility aliases from `init.lua` into C; implementation cleanup while preserving Lua compatibility. |
| !11781 | merged | Restores exported-PDU TLV layout/alignment documentation, including 32-bit alignment and deprecated length-tag semantics; documentation of an existing binary contract. |
| !11780 | merged | NAS 5GS reports unexpected padding with expert info and advances past it so dissection stays synchronized; good malformed-input recovery evidence. |
| !11779 | merged | Registers the assigned MDB DLT/WTAP encapsulation and hooks the MDB dissector through `wtap_encap`; normal table-driven capture encapsulation integration. |
| !11778 | merged | ASTERIX generator/dissector update tracks an upstream specification repository structural change; generated-spec maintenance. |
| !11777 | merged | Adds the Qt browser SSL-keylog dialog and removes the Lua implementation; review mainly covered UI details/cleanup, no new broad convention. |
| !11776 | merged | John Thacker stable-branch iSCSI fix replaces unsafe raw C-string pointer walking with bounds-checked TVBuff operations and adds IPv6 TargetAddress support; promoted as corroboration of TVBuff safety guidance. |
| !11775 | merged | Release-note typo correction; no engineering lesson. |
| !11774 | merged | Release-note updates for security and bug fixes; release bookkeeping. |
| !11773 | merged | Prepares 3.6.16 release notes/version content; release bookkeeping. |
| !11772 | merged | Prepares 4.0.8 release notes/version content; release bookkeeping. |
| !11771 | merged | Lua console prints returned values and supports `=` as a `return` shorthand like upstream Lua; REPL usability/compatibility feature. |
| !11770 | merged | Makes the Qt I/O console single-instance and, after Stig Bjørlykke review, restores/raises/activates an already-open minimized dialog; useful UI-state handling evidence. |
| !11769 | merged | Sharkd `info` exposes capture and encapsulation type lists analogous to editcap output; API feature with corresponding behavior coverage. |
| !11768 | merged | Keeps the Lua console alive across runtime reload while eliminating stale `lua_State` ownership; promoted to lifetime/reset-boundary guidance. |
| !11767 | merged | Improves detection of conflicting/suspicious `value_string` definitions and fixes findings; reinforces checker-driven metadata correctness. |
| !11766 | merged | Adds ordinal pcapng block numbering to the file reader, carrying forward earlier work from `!9140`; accepted capture-file metadata feature. |
| !11765 | merged | John Thacker fixes sharkd leaks identified by Coverity where `address_to_display(NULL, ...)` returns caller-owned storage; reinforces explicit ownership-contract review. |
| !11764 | merged | Moves E2AP/E2SM dissectors to v3-era specifications; large generated/protocol-version update, no new generic convention extracted. |
| !11763 | merged | João Valverde fixes fetched Lua 5.2 builds on Apple by using the macOS target, CMake's actual archiver/ranlib variables, and the configured SDK sysroot; corroborates existing platform build-environment guidance. |

All fifty selected MRs were merged, so no abandoned/superseded proposal needed down-weighting in this batch. High-authority accepted behavior from Guy Harris, João Valverde, John Thacker, and other maintainers was weighted by the substance of the accepted implementation and discussion. In particular, Guy Harris's authored-and-merged `!11805` was treated as the strongest parsing guidance in the batch.

## Frontier probe (not reviewed)

`!11762` exists in the corpus and is merged (`[Automatic update for 2023-08-20]`). It was inspected only to establish that the corpus continues below this batch and is **not** part of the reviewed set recorded above.
