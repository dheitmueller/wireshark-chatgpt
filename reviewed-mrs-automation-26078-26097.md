# Supplemental Reviewed Wireshark Merge Requests — !26078–!26097

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit used for this batch: `2261458afc3e287c111ffe13f534d9aaf5af07af`

Exactly twenty MRs were reviewed in this batch: **!26078 through !26097 inclusive**.

| MR | State / target | Review depth | Durable result / weighting |
|---|---|---|---|
| !26078 | merged, master | Medium | Pascal Quantin NAS 5GS fix makes the mutually exclusive TV versus TLV-E parameter formats actually mutually exclusive in control flow and uses `proto_tree_add_item_ret_uint()` to expose and consume the type from one fetch. Good parser correctness/API idiom; existing fetch-and-reuse guidance already covers the reusable part. |
| !26079 | merged, release-4.6 | Backport/corroboration | Backport of !26078. Corroboration only, not independent evidence. |
| !26080 | merged, release-4.4 | Backport/corroboration | Backport of !26078. Corroboration only, not independent evidence. |
| !26081 | merged, master | Discussion-focused | Radiotap display cleanup preserves existing filter fields while correcting misleading Info-column terminology for sounding PPDUs and uncaptured PSDUs. Anders Broman accepted the author's existing tree/value extraction approach. Useful compatibility/presentation precedent, but no new notebook rule was necessary. |
| !26082 | merged, master | Deep | John Thacker Windows process-launch/extcap work resolves registered file associations before `CreateProcess`, retains `PATHEXT` as the executable-discovery gate, gives same-name wrappers precedence, and updates documentation. Peter Wu supplied historical context on the prior unfinished approach and the Python/PATHEXT limitation. Strong Windows/extcap implementation evidence, but too platform-specific to promote beyond the ledger in this batch. |
| !26083 | merged, master | Deep | BitTorrent handshake extension-bit decoding. Jaap Keuter explicitly said it is good practice to show otherwise-undissected bits so use of a “new” bit becomes obvious. Accepted revision decodes known bits and exposes the remainder as unassigned. Promoted to `dissector-conventions.md`. |
| !26084 | merged, release-4.4 | Backport/corroboration | Backport of the Daintree timestamp-fraction fix from !26061, already reviewed and promoted in the previous batch. Corroboration only. |
| !26085 | open draft, master | Discussion-focused / down-weighted | New HyperDHT heuristic/stateful dissector includes a deterministic 38-frame capture, focused pytest coverage, positive/negative heuristic checks, request/response linking, and successful CI, but remains a draft. Useful testing corroboration only; implementation is not treated as accepted architecture. |
| !26086 | closed, master | Scanned / down-weighted | First IDN audio/StageFeed attempt closed quickly with a failed pipeline and no substantive review. Not accepted implementation evidence. |
| !26087 | closed draft, master | Scanned / down-weighted | Follow-on IDN iteration remained draft, failed CI, and closed after commit/whitespace churn without substantive maintainer resolution. Not accepted implementation evidence. |
| !26088 | closed, master | Scanned / down-weighted | c-ares callback-constness change closed unmerged with a failed pipeline and no substantive review. Not promoted. |
| !26089 | merged, master | Deep | Shared IEEE 802.15.4 Auxiliary Security Header decoder was generalized to accept caller-specific header-field registrations so MLE can expose `mle.aux_sec.*` rather than colliding with `wpan.aux_sec.*`. Anders Broman approved/merged. Promoted: share parsing mechanics without stealing another protocol's filter namespace. |
| !26090 | merged, release-4.6 | Backport/corroboration | Second backport of the Daintree timestamp-fraction fix already reviewed as !26061. Corroboration only. |
| !26091 | merged, master | Medium | Final accepted IDN audio/StageFeed submission after the failed/closed !26086 and !26087 iterations. Anders Broman merged the successful-pipeline revision. Its value here is chiefly outcome weighting: the merged successor, not the abandoned drafts, is the implementation exemplar. No distinct durable rule promoted. |
| !26092 | merged, master | Scanned | Martin Mathieson broad Python `ruff` cleanup across repository scripts. Valid current coding hygiene but mostly generic lint conformance; no new Wireshark-specific convention promoted. |
| !26093 | merged, master | Deep | John Thacker moves extcap control-input handling from Qt into shared extcap/capture code so structured toolbar/log/error/warning messages work for tshark and other frontends too. It also explains why arbitrary stderr/stdout cannot simply be treated as the framed sync protocol. Promoted to `architecture.md`. |
| !26094 | merged, master | Medium | John Thacker fixes the Windows extcap example wrapper to locate its sibling Python script relative to the batch file (`%~dp0`) instead of assuming the current working directory is the extcap directory. Good launcher-path precedent; retained in ledger rather than promoted separately. |
| !26095 | merged, master | Deep/corroboration | John Thacker extracts sync-pipe reading into reusable capture-layer source/header code, enabling !26093's UI-independent extcap control handling. Strong corroboration of the shared-control-plane architecture and promoted together with !26093. |
| !26096 | merged, master | Deep / highest authority | Authored and merged by Guy Harris. ZigBee Touchlink clears a commissioning map at dissection init because the map survives while all file-scope objects referenced by its entries are freed before redissection. This prevents dangling-pointer/UAF crashes exposed by a capture that queues redissection after a later DSB. Promoted to `architecture.md` as a high-confidence allocator-scope/container-lifetime rule. |
| !26097 | merged, master | Deep/corroboration | John Thacker builds extcap-base structured logging and control callbacks on the dedicated control pipes; Anders Broman merged it. Discussion explicitly notes stdout must retain its normal semantics when extcaps run directly in capture mode, so redirection should occur only when a dedicated pipe establishes the contract. Promoted as corroboration of the !26093 shared-control-plane rule. |

## Weighting notes

Merged master-branch changes received the greatest weight. !26079, !26080, !26084, and !26090 are release backports and were treated as corroboration rather than independent evidence. Open draft !26085 and closed/unmerged !26086–!26088 were explicitly down-weighted. For the IDN sequence, merged !26091 is the accepted implementation exemplar rather than its failed/closed predecessors.

!26096 receives exceptional maintainer weight because it was both authored and merged by Guy Harris and states the allocator/lifetime rationale directly. John Thacker's authored/merged !26082, !26093, !26094, and !26095 and his authored !26097 also receive strong maintainer weight; Anders Broman, Jaap Keuter, Peter Wu, Pascal Quantin, and Martin Mathieson review/merge evidence was weighted according to outcome and context.

## Notebook changes from this batch

- `dissector-conventions.md`: expose reserved/unassigned bits in extension bitfields so future use becomes visible; let shared structure decoders accept caller-specific field registrations so filter namespaces preserve protocol ownership.
- `architecture.md`: clear longer-lived containers when an allocator scope holding their referenced objects is reset across redissection; keep UI-independent extcap control/status handling in shared extcap/capture code and use a dedicated framed control channel rather than overloading ordinary stdout/stderr.
- Existing fetch-and-reuse, testing, compatibility, CI, and backport guidance was corroborated but not duplicated.

Notebook commits created before this ledger:

- `e885c5df022f105b1dd1c5f1422d5472f3eda080` — bitfield visibility and shared-filter-namespace conventions.
- `9d84a4d6759e2ebd90c3a104fa5c395e8e18811b` — redissection lifetime and extcap control-plane architecture.
