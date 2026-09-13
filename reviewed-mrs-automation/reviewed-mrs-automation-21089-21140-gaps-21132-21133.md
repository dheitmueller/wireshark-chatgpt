# Wireshark MR automation review: next 50 through !21089

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest available unreviewed MRs toward older MRs

## Exact MRs reviewed

`!21140 !21139 !21138 !21137 !21136 !21135 !21134 !21131 !21130 !21129 !21128 !21127 !21126 !21125 !21124 !21123 !21122 !21121 !21120 !21119 !21118 !21117 !21116 !21115 !21114 !21113 !21112 !21111 !21110 !21109 !21108 !21107 !21106 !21105 !21104 !21103 !21102 !21101 !21100 !21099 !21098 !21097 !21096 !21095 !21094 !21093 !21092 !21091 !21090 !21089`

Count: **50**.

`!21133` and `!21132` are not present in the pinned corpus, so they are not counted as reviewed and are not silently represented by the numeric span. The historical !17571–!17620 batch remains part of the reviewed set and was preserved/countable during selection. Selection was based on the union of all per-run automation ledgers and `reviewed-mrs.md`, not on assumptions that numeric ranges were complete.

## Review weighting and durable findings

- **!21101** (merged master; stable backports !21122/!21123/!21124): `cli_main` keeps ownership bookkeeping independent of a mutable `argv` view. Because the callee may legally rewrite pointer entries, cleanup cannot depend on the post-call array. Promoted to `ownership-mutation-conventions.md`.
- **!21100** (merged master; stable backport !21114): application flavor must be established before `configuration_init()` derives flavor-dependent paths/state. Promoted to `initialization-lifecycle-conventions.md`.
- **!21128** (merged accepted fix): replace wildcard Qt disconnection with the exact signal/slot relationship; broad disconnection can remove `destroyed` and other framework/lifetime-management connections. Promoted to `qt-signal-connection-conventions.md`. **!21126** is a closed/superseded sibling and was explicitly down-weighted.
- **!21138 / !21137** (merged): temporary mutation of shared `packet_info` transport fields must be restored after nested RoCE/iWARP dissection so Follow Stream and other outer-layer consumers retain the real transport context. Strong corroboration of the existing push/pop context rule in `dissector-context-flow-conventions.md`.
- **!21130** (merged, substantive Martin Mathieson review) plus **!21134/!21135**: `check_typed_item_calls.py` exposed a field-registration mask/value-table mismatch. Strong corroboration that checker findings can identify semantic `hf_` definition defects, not merely mechanical call-site issues.
- **!21139** (merged): validate file-derived frame-table size against actual file extent before allocating; strong corroboration of existing input-resource/allocation-bound rules.
- **!21136 / !21129** (merged): error paths must release temporary/partially built allocations; corroborates established ownership rules.
- **!21117** (merged): shared keytab-reading functionality moved out of dissector-specific implementation into epan; corroborates neutral-layer ownership for capabilities consumed outside one dissector.
- **!21115** is merged but its temporary `packet_info` mutation was later corrected by !21137, so the accepted later fix carries more architectural weight than the original implementation detail.
- **!21093** eventually merged after extended design discussion around Procmon Wiretap/Stratoshark event transport. Its accepted use of file-type-specific event records is useful architecture evidence, but no new notebook rule was added because the existing capture/decoder boundary guidance already covers the durable separation.
- **!21091 / !21092** (merged master/stable): absence of Falco source plugins is an expected discovery outcome and returns success; corroborates existing CLI/extcap exit-status conventions.
- Release-preparation, generated-update, stable-backport, packaging, and protocol-feature MRs in this batch were reviewed but did not produce distinct durable conventions where stronger notebook rules already existed.

## Notebook changes

- Extended `ownership-mutation-conventions.md` with the independent-ownership-ledger rule from !21101 and its stable backports.
- Extended `initialization-lifecycle-conventions.md` with the rule to establish mode/flavor selectors before initialization derives dependent state, from !21100/!21114.
- Added `qt-signal-connection-conventions.md` with the exact-disconnect rule from accepted !21128 while explicitly down-weighting closed !21126.

No change to `reviewed-mrs.md` was required for correctness; this exact per-run ledger is authoritative for this run.
