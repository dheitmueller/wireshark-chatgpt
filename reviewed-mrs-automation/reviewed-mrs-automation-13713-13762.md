# Wireshark MR review run: !13762 through !13713

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `8b9508a1231f8c621317b7edd28ec17bb30928dd`

Selection was reconciled against all available review tracking in `reviewed-mrs.md`, the aggregate automation tracking, and the per-run ledger inventory under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains part of the already-reviewed set. The preceding exact ledger covered !13812 through !13763 and explicitly treated !13762 only as a frontier probe, not as reviewed. The corpus remained at the same commit for this run, so exact set subtraction selected the fifty highest-numbered previously unreviewed corpus MRs listed below.

## Exact reviewed set

!13762, !13761, !13760, !13759, !13758, !13757, !13756, !13755, !13754, !13753,
!13752, !13751, !13750, !13749, !13748, !13747, !13746, !13745, !13744, !13743,
!13742, !13741, !13740, !13739, !13738, !13737, !13736, !13735, !13734, !13733,
!13732, !13731, !13730, !13729, !13728, !13727, !13726, !13725, !13724, !13723,
!13722, !13721, !13720, !13719, !13718, !13717, !13716, !13715, !13714, !13713.

Exactly 50 MRs were reviewed. **All 50 were merged.** Master changes were weighted more heavily than stable cherry-picks/retries, and substantive maintainer review was weighted more heavily than mechanical approvals.

## Durable findings promoted

- **!13745 — derive IPC receive capacity from the framed-message contract.** John Thacker's merged master change ties `PIPE_BUF_SIZE` directly to `SP_MAX_MSG_LEN` plus framing overhead instead of maintaining two independent constants. The MR explicitly notes that increasing the protocol maximum without the receive buffer could cause a segfault. `subprocess-ipc-lifecycle-conventions.md` now records the coupled-size invariant.

- **!13743, with !13738/!13720/!13715 — design capture capability IPC for extension.** This merged capture/welcome-screen series moves interface and capability information through structured JSON while a single dumpcap child can continue delivering statistics. Guy Harris's review receives especially high weight: he cautioned that future libpcap can expose extensible settable options and pcapng-style interface characteristics and that the design should make no fixed assumptions about the property set. `subprocess-ipc-lifecycle-conventions.md` now records the extensible-schema rule.

- **!13756 — semantically meaningful structural tree nodes should be real fields.** John Thacker's merged master SCTP change replaces a text-only chunk subtree with an `FT_NONE` `sctp.chunk` field so users can use natural existence/count filters such as `count(sctp.chunk)`. `protocol-tree-hierarchy-conventions.md` now records the distinction between structural semantic fields and purely presentational text nodes.

- **!13755 — wrappers should not impose stricter bounds behavior than common proto-item machinery.** John Thacker's merged master core change removes an early text-subtree tvbuff existence check because shared `FT_NONE`/`FT_PROTOCOL` item creation already clips the visible item to captured data. With a truncated capture, the structural node can be added and the exception occurs only when dissection actually asks for an unavailable byte. `protocol-tree-hierarchy-conventions.md` now records this common captured-length semantic.

- **!13744 — valid zero-length elements must not manufacture child fields.** The merged GTP fix follows TS 29.060 semantics where a zero MS Network Capability length means the capability is not included. The old decoder still displayed bits, producing misleading values. Review concluded that the enclosing zero-length item was sufficient. `protocol-tree-hierarchy-conventions.md` now records the valid-empty versus truncation distinction.

- **!13750 — reports that promise current configuration must initialize current configuration.** John Thacker's merged master fix makes profile-sensitive `tshark -G protocols`, `heuristic-decodes`, and `decodes` explicitly load settings before emitting output. They had run through an early startup path and silently reported defaults. New `cli-report-state-conventions.md` records the semantic-initialization rule and a non-default-profile testing practice.

- **!13729 — centralize packet-provider semantics shared by frontends.** This merged master change was authored and merged by Guy Harris and therefore receives especially high weight. Several almost-identical frame-timestamp callbacks become one `cap_file_provider_get_frame_ts()` helper covering the reference frame, cached previous frames, ordinary sequence lookup, and timestamp validity. New `packet-provider-conventions.md` records that capture-file provider semantics belong in a shared provider layer rather than frontend copies that can drift.

## Strong corroborating and review findings retained without duplicating notebook rules

- **!13760/!13759/!13758/!13757** are stable/master forms of the SCTP Qt signal/slot fix. The old string-based `SIGNAL`/`SLOT` signature used `QString&` even though the actual signal passed `QString` by value; Qt6 reported that no such signal existed. This reinforces exact callback/signature contracts and auditing all call/connect sites when an API signature changes.
- **!13754** received direct Jaap Keuter review that per-bit display texts were far too long to be useful and would not fit normal output. The author replaced them with concise semantic names. This reinforces keeping protocol-tree field/value labels readable and reserving long normative prose for descriptions/documentation rather than inline values.
- **!13752** contains detailed Guy Harris discussion of macOS group/ACL behavior around `access_bpf`. It is useful authoritative platform evidence but is too macOS-specific for a new project-wide convention here.
- **!13747** is a large merged revert of an earlier plugin API redesign after prolonged discussion. The MR explicitly demanded clearer scope, roadmap, cost/benefit reasoning, and broad agreement before a project-wide plugin architecture change. It is strong review/process evidence for socializing broad API redesigns, but the discussion was contested enough that it is retained in the run ledger rather than promoted as a hard universal rule.
- **!13734/!13733/!13731** are stable/master forms of the interface-list auto-sort fix. Using Qt layout-change notifications instead of a full model reset preserves the selected interface while still allowing resorting. This independently corroborates the existing `ui-model-state-conventions.md` rule to choose model notifications that match the semantic size of the change and preserve identity/selection.
- **!13745** also reinforces avoiding duplicated constants for one semantic limit: the accepted formula makes a future maximum-size change update the receive side automatically.
- **!13739** fixes LLDP dissector return handling so a subordinate end-of-PDU helper cannot make the top-level dissector return an error value merely because the terminating TLV was reached. This corroborates treating helper return values according to the caller's contract rather than propagating them mechanically.
- **!13749/!13732/!13722/!13721** are IEEE 802.11ah RPS parsing fixes and follow-ups covering multiple consecutive RAW assignments and slot definition details; they are useful protocol-specific parser evidence but did not establish a stronger project-wide rule.
- **!13748** fixes `asn2wrs` operation without an optional `-L` flag, reinforcing testing generator/tools with optional arguments omitted rather than only the maintainer's usual invocation.
- **!13738/!13723/!13720/!13715/!13710-family work** incrementally separates, serializes, caches, and combines capture-interface/capability discovery. The strongest durable point from this series is the extensible structured IPC rule promoted above.
- **!13725** reverts an incorrect Debian codec-plugin placement change; it is useful packaging evidence that distro-specific layout changes should be validated against the actual package policy before being generalized.
- **!13724/!13728** contain focused NR/RLC graph fixes, including preserving RAT in UE matching and correcting initial time-axis scale; useful UI/statistics correctness evidence but not stronger than existing state/model conventions.
- **!13719/!13718/!13717/!13716** are automated data-update retries/results and were weighted as maintenance evidence rather than architectural guidance.
- **!13713** replaces manual `proto_item_set_len(item, end-start)` bookkeeping with `proto_item_set_end(item, tvb, offset)`, reinforcing use of the API that directly expresses the intended endpoint and removes redundant start-offset state.
- **!13762** is a release-4.2 Qt profile-menu ownership/lifetime backport; the corresponding master behavior remains stronger evidence than this stable cherry-pick.

The remaining MRs were protocol-specific correctness fixes, stable backports, documentation/link maintenance, generated-data refreshes, packaging fixes, and focused UI/build cleanups that did not establish a stronger durable project-wide convention than the notebook already records.

## Notebook commits from this run

- `83ae1d3f3901213e1cd68ea2e05f33081ffd1137` — add IPC buffer-size invariant and Guy Harris's extensible capture-capability schema guidance.
- `bfcb24fdce428ce42e14642572c9d4adbf115efa` — add semantic `FT_NONE` structural nodes, common truncated-capture item semantics, and valid-zero-length field handling.
- `234b49d2755e98f1655e89deb674eec4bfc5d38b` — add profile/current-configuration initialization rules for CLI reports.
- `a2b287d20b88578e743b29873a976f7a00833ab7` — add Guy Harris's shared packet-provider semantics rule.

## Frontier

`mr_13712.json` exists in corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054` and is closed. It was inspected only to verify that the corpus continues and was **not** counted as reviewed. Therefore the corpus is not exhausted; absent newly scraped higher-numbered unreviewed material, !13712 is the next descending candidate.
