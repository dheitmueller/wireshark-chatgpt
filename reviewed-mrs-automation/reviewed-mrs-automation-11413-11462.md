# Wireshark MR automation review: !11462 through !11413

- Reviewed on: 2026-09-24
- Model: GPT-5.6 Sol
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `645e4f37a1956721413bc96b536dd70d873ba448`
- Review direction: descending from newest available unreviewed MR toward older MRs
- Reviewed in this run: **50 MRs**
- Outcome mix: **49 merged, 1 closed/unmerged (!11442)**

## Selection and duplicate-avoidance

Before selecting the batch, the already-reviewed set was reconciled from the available notebook tracking: `reviewed-mrs.md`, the aggregate `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the per-run ledgers under `reviewed-mrs-automation/`. The historical **!17571-!17620** batch remains part of the reviewed set. An MR was considered reviewed only when it was explicitly represented by the tracking data; interval coverage was not inferred merely from neighboring ledger ranges.

The prior mention of **!11462** was only a frontier/existence probe after the preceding run and was not itself a review. It therefore remained eligible here. The fifty highest-numbered corpus MRs not already in the reviewed set were selected individually.

Exact MRs reviewed in this run:

`!11462, !11461, !11460, !11459, !11458, !11457, !11456, !11455, !11454, !11453, !11452, !11451, !11450, !11449, !11448, !11447, !11446, !11445, !11444, !11443, !11442, !11441, !11440, !11439, !11438, !11437, !11436, !11435, !11434, !11433, !11432, !11431, !11430, !11429, !11428, !11427, !11426, !11425, !11424, !11423, !11422, !11421, !11420, !11419, !11418, !11417, !11416, !11415, !11414, !11413`

## Per-MR review ledger

| MR | Status / depth | Review result |
|---|---|---|
| !11462 | merged, stable backport / scanned | Qt folder-name edit behavior: update only the relevant radio selection rather than running a broad widget refresh that can disturb text/cursor state. Stable sibling of the master fix. |
| !11461 | merged, release-4.0 / deep corroboration | Gerald Combs CBOR recursion hardening. Adds a finite protocol recursion limit, expert diagnostic, and saves/restores protocol-local depth around recursive descent. Strong corroboration of existing recursion/depth notebook policy. |
| !11460 | merged, release-4.0 / scanned | Qt folder-name edit behavior backport; same narrow-state-update pattern as !11457/!11462. |
| !11459 | merged, release-4.0 / deep corroboration | Guy Harris Windows 11 22H2 detection backport. Reinforces deriving what the platform API can actually establish from build/version data rather than overstating system identity. |
| !11458 | merged / scanned | Removes deprecated display-filter `~=` handling; API/language cleanup with no new durable rule beyond existing deprecation guidance. |
| !11457 | merged / deep | Master Qt folder-name text-entry fix. Avoids an overly broad `updateWidgets()` call when a direct radio-state update preserves the user's active edit/cursor state. Useful UI-state exemplar. |
| !11456 | merged / deep corroboration | Guy Harris master Windows version detection update. Uses build thresholds rather than one exact release build and discusses limits/alternatives of Windows version-brand APIs. Existing platform-portability guidance already covers the general rule. |
| !11455 | merged / scanned | Adds default log-column preferences without overwriting existing hard-coded custom-column behavior. Reinforces preserving configuration precedence/default semantics. |
| !11454 | merged, stable / scanned | XnAP test backport; testing maintenance, no new project-wide convention. |
| !11453 | merged / scanned | XnAP test addition/update; useful protocol coverage but no distinct durable rule. |
| !11452 | merged, stable / scanned | XnAP test correction backport; no distinct new convention. |
| !11451 | merged / scanned | XnAP/Coverity-driven test/code correction; corroborates static-analysis review practice. |
| !11450 | merged / deep corroboration | New VP9 RTP payload dissector. Alexis La Goutte requested both release-note coverage and a representative capture; author supplied both before merge. Reinforces existing new-dissector submission/test-vector expectations. |
| !11449 | merged / scanned | Dumpcap Linux permission diagnostics point users toward the appropriate capabilities rather than a generic permission failure. Good actionable-diagnostic exemplar. |
| !11448 | merged / scanned | Debian exported-symbol maintenance; packaging/API bookkeeping, no new convention. |
| !11447 | merged / scanned | Couchbase ReplicaRead subdocument flag support; straightforward protocol extension. |
| !11446 | merged / deep corroboration | Coverity exposed a dead NTLMSSP condition. Martin Mathieson consulted the author/history to verify the intended decryption sequencing before deleting the branch, rather than treating analyzer output mechanically. Corroborates existing analyzer-review guidance. |
| !11445 | merged / **deep, promoted** | John Thacker crash fix: `ws_label_strcpy()` has `strlcpy()`-style would-have-written return semantics, while its caller needed actual bytes copied for further appending. Promoted to `buffer-length-return-conventions.md`. |
| !11444 | merged / scanned | Automatic update; routine generated/registry maintenance. |
| !11443 | merged / scanned | WSUG preference/documentation image update; documentation maintenance. |
| !11442 | **closed, unmerged** / down-weighted | Automatic update proposal that did not merge and was superseded by accepted update work. No durable implementation evidence extracted. |
| !11441 | merged, release-3.6 / scanned | Automatic update on stable branch; routine maintenance. |
| !11440 | merged, release-4.0 / scanned | Automatic update on stable branch; routine maintenance. |
| !11439 | merged, stable / scanned | Qt marked-packet displayed-count fix backport. The export behavior was already correct; UI count/status presentation was wrong. |
| !11438 | merged, stable / scanned | Sibling marked-packet displayed-count backport. |
| !11437 | merged / deep | Adds Qt export option concerning depended-upon packets and documents limitations of current range/dependency semantics near range boundaries. Useful UI/export behavior evidence, but no new general rule beyond existing dependency handling. |
| !11436 | merged / scanned | Master marked-packet displayed-count correction. |
| !11435 | merged, release-3.6 / corroboration | QUIC/TLS `max_ack_delay` GREASE display fix stable backport; supports !11428/!11433 predicate rule. |
| !11434 | merged / deep corroboration | Martin Mathieson fixes Coverity-reported unreachable/double-return code in ASN.1/H248 dissectors. Static-analysis cleanup consistent with existing checker guidance. |
| !11433 | merged, release-3.6 / **deep, promoted** | Replaces ad-hoc GREASE tests with named predicates and documents that an old expression could falsely classify a non-GREASE value. Supports `protocol-predicate-conventions.md`. |
| !11432 | merged / scanned | WSUG Foundation note/documentation update. |
| !11431 | merged / scanned | TIFF division-by-zero defense. Straightforward malformed-input arithmetic guard, already covered by hostile-input arithmetic guidance. |
| !11430 | merged / **deep, promoted** | John Thacker DICOM Export Objects follow-up centralizes long-lived allocations in the owning tap/UI side, makes producer data borrowed/const, and narrows transient producer storage to packet lifetime. Promoted to `tap-export-ownership-conventions.md`. |
| !11429 | merged, release-4.0 / deep corroboration | Stable QUIC GREASE predicate fix plus prerequisite macro cleanup. Supports the protocol-predicate convention and shows backports may need prerequisite semantic refactors rather than blind cherry-picks. |
| !11428 | merged / **deep, promoted** | QUIC GREASE predicate guarded before unsigned subtraction/modulo; John Thacker explicitly notes wraparound is not desired. Promoted to `protocol-predicate-conventions.md`. |
| !11427 | merged / deep | PROXYv2 coalesced-packet support with representative captures. John Thacker caught unconditional TCP child dispatch and required it to be conditional on the proxied transport. Author explicitly documented remaining two-pass limitations later addressed by newer PROXY work. |
| !11426 | merged / scanned | IRC CTCP modeled as a Name Only protocol beneath IRC; straightforward protocol-organization change. |
| !11425 | merged / deep corroboration | John Thacker DICOM leak fix: strings retained by file-scoped state should use a matching lifetime. Supports the ownership/lifetime convention without needing a duplicate rule. |
| !11424 | merged, release-4.0 / deep corroboration | RTP Decode As path created no conversation, so packets were decoded independently. Fix establishes the same conversation prerequisite alternate entry paths get through signaling. Corroborates existing conversation API guidance. |
| !11423 | merged, release-4.0 / corroboration | Backport of DICOM Export Objects ownership crash fix from !11422. |
| !11422 | merged / **deep, promoted** | DICOM handed wmem file-scope payload directly to Export Objects, which later `g_free()`d it and crashed. Accepted fix copies into the consumer's allocator domain. John Thacker also requested the issue-closing reference in the commit body. Promoted to `tap-export-ownership-conventions.md`. |
| !11421 | merged / **deep, promoted** | Protobuf schema loading replaced repeated `g_slist_append()` traversal with prepend when order is irrelevant and `GQueue` when tail order matters. Promoted to `container-build-performance-conventions.md`. |
| !11420 | merged / scanned | MAC-LTE `value_string` conflict fixes. Corroborates existing checker/value-table correctness policy. |
| !11419 | merged / deep | RDPUDP global sequence-number calculation correction around wrap/workaround transitions; useful state-machine correctness evidence but no unique project-wide rule. |
| !11418 | merged / scanned | DNS CHAOS-class A-record support; ordinary protocol extension. |
| !11417 | merged / scanned | Debian missing-symbol maintenance. |
| !11416 | merged, release-3.6 / scanned | CFM TEST-ID TLV workaround for a specification length defect; compatibility fix, no broader rule beyond existing real-world/specification compatibility guidance. |
| !11415 | merged / deep corroboration | John Thacker T.38 reassembly fix supports >2 data items/frame, avoids advancing sequential counters again on redissection, preserves first-pass values, and notes that frame number is not a sufficient ID if multiple reassemblies can begin in one frame. Strong corroboration of existing `stateful-reassembly-conventions.md`. |
| !11414 | merged / scanned | WSUG title-casing correction. |
| !11413 | merged / deep corroboration | ISIS SRv6 RFC 9353 update with a representative capture. Jaap Keuter's review removes `_U_` annotations from parameters that are actually used and tightens signatures. Reinforces capture-backed protocol updates and ordinary compiler-hygiene conventions. |

## Durable notebook changes

This run added four focused notebook files for lessons not already adequately represented by stronger/later reviewed material:

- `buffer-length-return-conventions.md` — commit `caf923805a604fd25912900d3b50b49ed998e7b5`: distinguish would-have-written/logical length from bytes actually materialized when a copy helper's result feeds later offsets or concatenation (!11445).
- `tap-export-ownership-conventions.md` — commit `422b2cfce387d05ca4cb67e687f352823167cf99`: make allocator-family and ownership transfer explicit at dissector/tap/UI/export boundaries and narrow producer lifetime when the consumer copies (!11422, !11423, !11425, !11430).
- `container-build-performance-conventions.md` — commit `6d996ff2bf88e3f531b6a7109939f23812a66d8f`: avoid accidental O(n^2) singly-linked-list construction; choose prepend vs queue from ordering requirements (!11421).
- `protocol-predicate-conventions.md` — commit `d62a7867bf6d3e4e28fc438700d7cfd31e3437d8`: validate the arithmetic domain before subtraction/modulo in specification-defined value predicates and centralize such predicates in named helpers (!11428, !11429, !11433, with !11435 as stable corroboration).

Several additional high-quality findings were intentionally treated as corroboration rather than creating duplicate notebook material. In particular, Gerald Combs's CBOR recursion bound in !11461 reinforces `parser-recursion-conventions.md`, including stack-like save/set/restore behavior for protocol-local depth; John Thacker's T.38 work in !11415 reinforces the existing redissection-order/state rules in `stateful-reassembly-conventions.md`; !11424 reinforces alternate-entry-path conversation initialization; and !11450 reinforces capture/release-note expectations for new dissectors.

## Frontier check

After completing these fifty reviews, **!11412** was inspected only as an existence/frontier probe. It exists in the corpus and is merged (`DHCPFO: Improve handling of vendor-specific options`). It was **not** reviewed or counted in this run and remains eligible for the next descending batch absent newly scraped higher-numbered unreviewed material.

The corpus therefore has **not** run out of previously unreviewed MRs, and no scraper-restart notification is required for this run.
