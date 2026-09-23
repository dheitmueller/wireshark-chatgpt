# Wireshark MR automation review: !11963 through !12012

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `5cd00cc98fda7c83fd26dd35e2ebbf7f20063fc2`
- Direction: descending from the newest available previously unreviewed MR.
- Selection method: reconstructed the already-reviewed set from the available `reviewed-mrs.md` tracking and the per-run files under `reviewed-mrs-automation/`; did not infer review status merely from numeric ranges. The historical !17571-!17620 batch remains preserved/countable. The previous run's mention of !12012 was only a frontier check and did not count as review.
- Batch size: exactly 50 MRs.
- Status weighting: 46 merged, 4 closed/unmerged (!12003, !11972, !11964, !11963). Closed work was down-weighted; merged work later explicitly reverted or superseded was also treated as historical evidence rather than accepted current behavior.

## Exact reviewed set

| MR | Status / review weight | Review note |
| --- | --- | --- |
| !12012 | Merged / corroborating | `strptime` `%s` portability fix avoids assuming 64-bit `time_t`, including 32-bit supported environments; reinforces existing C/platform portability guidance. |
| !12011 | Merged / scanned | ALC/LCT Coverity fix guards optional `data_exchange` before ATSC 3-specific dereferences. Straightforward malformed/context robustness fix. |
| !12010 | Merged / scanned | Gerald Combs CI adjustment pins/fixes the Windows Qt version. Build-infrastructure maintenance. |
| !12009 | Merged backport / lower independent weight | Backports display-filter absolute-time fixes; substantive semantics are weighted at the master changes. |
| !12008 | Merged revert / high evidentiary value over !12007 | Reverts the larger NSIS compression dictionary after the Arm64 builder failed `deflateInit()`. Demonstrates that nominal tool support does not replace validation on the actual build matrix. |
| !12007 | Merged but immediately reverted / superseded | Increased NSIS dictionary size based on nominal NSIS capability; superseded by !12008 after Arm64 failure. Not treated as accepted lasting policy. |
| !12006 | Merged / scanned | Applies the application style to the overlay scroll bar so child-widget dark-mode rendering follows the active Qt style on Windows/Linux. |
| !12005 | Merged / corroborating | Changes expert-info test capture to exercise Error/Warning/Note/Chat severities and explicitly anticipates the later completed-reassembly retransmission change; reinforces representative regression vectors. |
| !12004 | Merged master / deep / promoted | John Thacker removes RTP's static four-record tap scratch array and allocates one zeroed `rtp_info` per dissection from `pinfo->pool`; arbitrary per-frame callback caps are unsafe. Added to `tap-data-contract-conventions.md`. |
| !12003 | Closed/unmerged / down-weighted | Aruba RADIUS dictionary expansion proposal. No accepted implementation precedent extracted from the closed MR. |
| !12002 | Merged master / deep / highest authority / promoted | Guy Harris removes an `errno == 0` test after parsing success because ambient `errno` is not guaranteed to be zero. Added to `errno-contract-conventions.md`. |
| !12001 | Merged / corroborating | IMF date parsing accepts both space and tab because RFC 5234 `WSP` permits both; standards grammar should be implemented as specified rather than as the most common spelling. |
| !12000 | Merged master / deep / promoted | John Thacker changes RTP tap `info_payload_len` to exclude RTP padding while retaining full raw data/framing metadata; fixes payload export and codec input. Added to `tap-data-contract-conventions.md`. |
| !11999 | Merged / corroborating | Makes legacy `ENC_RFC_1123`/`ENC_RFC_822` spellings aliases of `ENC_IMF_DATE_TIME`, preserving compatibility while converging on accurate semantics. |
| !11998 | Merged master / deep / highest authority / promoted | Guy Harris cleans up string byte/time parsing: direct `NULL` failure instead of ambient `errno`, IMF semantics rather than misleading RFC-specific modes, RFC-defined short-year handling, and matching Lua tests. Added to `errno-contract-conventions.md`; also corroborates parser/encoding conventions. |
| !11997 | Merged / scanned | Qt dark-mode handling uses `QStyleHints::colorScheme` when available and the appropriate Windows style workaround. Platform/UI behavior change. |
| !11996 | Merged master / deep / corroborating | WebSocket fragmented-message decompression/follow-stream fix. John Thacker review also corrects a protocol-data key limit to `G_MAXUINT32` because `p_add_proto_data` takes a 32-bit key; reinforces exact API-domain typing. |
| !11995 | Merged cherry-pick / lower independent weight | CI file-search fix backport/cherry-pick. |
| !11994 | Merged cherry-pick / lower independent weight | Sibling CI file-search fix backport/cherry-pick. |
| !11993 | Merged / scanned | BTATT label/filter consistency cleanup. |
| !11992 | Merged / corroborating | PCEP fixes an IPv6 LSP Extended Tunnel ID that is 16 octets rather than `UINT64`; includes IPv4/IPv6 sample captures. Reinforces matching registered field representation to wire width and supplying captures for protocol fixes. |
| !11991 | Merged / scanned | Master CI file-search correction corresponding to the nearby cherry-picks. |
| !11990 | Merged / scanned | Regenerates Wiretap introspection enums to include `WTAP_ENCAP_MDB`; generated-enum maintenance. |
| !11989 | Merged / corroborating | Gives `WTAP_TSPREC` enumerators explicit numeric values because their numbers are the precision digits and must be visible to generated introspection. Reinforces existing timestamp-precision domain conventions. |
| !11988 | Merged / corroborating | TACACS+ adds expert warning for the RFC 8907-deprecated unencrypted flag. Standards-forbidden/deprecated wire behavior should remain dissectable while being surfaced diagnostically. |
| !11987 | Merged stable backport / lower independent weight | Backport of !11979 RTPS signed-offset overflow fix. |
| !11986 | Merged stable backport / lower independent weight | Sibling backport of !11979. |
| !11985 | Merged / scanned | Frame arrival time is exposed in local/UTC/Unix forms without a preference whose only effect was hiding useful representations. |
| !11984 | Merged / corroborating | Display-filter absolute-time representation retains all nine nanosecond fractional digits instead of stripping significant zeroes; corroborates existing timestamp-formatting rules. |
| !11983 | Merged stable backport / lower independent weight | Backport of !11966 fuzz-job failure-propagation correction. |
| !11982 | Merged stable backport / lower independent weight | Sibling backport of !11966. |
| !11981 | Merged / scanned | Debian packaging symbol list update. |
| !11980 | Merged / accepted version | AWDL DataPathStateTLV additions and channel-number fix; weighted above the closed draft !11972. |
| !11979 | Merged master / deep / corroborating | John Thacker fixes RTPS signed offset overflow where adding an unsigned wire value could wrap a signed relative offset backward and cause excessive looping. Reinforces existing arithmetic-safety guidance; backports !11986/!11987 confirm importance. |
| !11978 | Merged revert / accepted over !11976 | Reverts the preceding FlexRay NFI inversion after confirming the protocol flag semantics; the earlier change is not treated as current precedent. |
| !11977 | Merged / corroborating | Save As rescanning frees replaced filename/linktype state, resets `wtap_rec` per block, and cleans dumper parameters. Reinforces ownership/cleanup discipline for reusable I/O state. |
| !11976 | Merged but reverted / superseded | FlexRay NFI inversion fix; explicitly undone by !11978 after protocol semantics were rechecked. |
| !11975 | Merged / scanned | `hf_` blurb cleanup. Presentation/documentation maintenance. |
| !11974 | Merged backport / corroborating | Corrects capture-file `prev_dis` state so only an actually displayed packet becomes the previous displayed packet when reference times change. |
| !11973 | Merged master / deep / highest authority / promoted | Guy Harris fixes `mktime_utc()` because `-1` is both an error sentinel and the valid timestamp one second before the Unix epoch; the wrapper now pairs the return value with an explicit normalized `errno` contract. Added to `errno-contract-conventions.md`. |
| !11972 | Closed draft / down-weighted | Earlier AWDL DataPathStateTLV draft; superseded in practical weight by merged !11980. |
| !11971 | Merged / superseded contract | Guy Harris initially saves/restores `errno` around successful `timegm()` implementations that may modify it; the later !11973 deliberately replaces this with a stronger normalized wrapper contract. Useful historical evidence, but !11973 controls. |
| !11970 | Merged / deep UI evidence | Fixes Qt completer event ordering: Return can reach `applyDisplayFilter()` before the completer has installed/syntax-checked its selected text, so the old filter must not be applied opportunistically. Reinforces event-order-aware UI state handling. |
| !11969 | Merged revert / corroborating | Reverts a display-filter representation cleanup because behavior was not equivalent and the test suite exposed failures. Behavior-preserving refactors must be judged by tests, not visual simplification. |
| !11968 | Merged / scanned | Corrects IEEE 802.11 EHT information-element names. |
| !11967 | Merged master / deep / highest-authority review / promoted | Guy Harris points out that MAC-48 display behavior is not Ethernet-specific and belongs in the common protocol-tree layer with `proto_tree_add_*` naming; accepted implementation becomes `proto_tree_add_mac48_detail()` in `epan/proto.c`. Added to `shared-helper-ownership-conventions.md`. |
| !11966 | Merged master / deep / corroborating | Gerald Combs fixes fuzz CI so the primary fuzz command still makes the job fail while exporting state for `after_script`; stable backports !11982/!11983 follow. Strongly corroborates the existing `ci-tooling-conventions.md` outcome/failure-propagation rules. |
| !11965 | Merged / accepted version | DOCSIS MDD TLV dissection improvements add unknown-TLV visibility/expert diagnostics and field corrections; accepted work is weighted above the two immediately preceding closed attempts. |
| !11964 | Closed/unmerged / down-weighted | Earlier DOCSIS MDD TLV attempt in the same sequence; no independent accepted precedent. |
| !11963 | Closed/unmerged / down-weighted | Earlier DOCSIS MDD TLV attempt in the same sequence; no independent accepted precedent. |

## Durable notebook changes from this batch

### `errno` is meaningful only as part of an explicit return contract

The Guy Harris sequence !11971, !11973, !11998, and !12002 is unusually strong evidence. Ambient `errno` cannot be treated as a durable success indicator: successful platform helpers may modify it, later calls may overwrite it, and stale nonzero values do not invalidate a separately successful parse. Prefer direct failure returns for parser/conversion APIs. If a legitimate value collides with a traditional sentinel—as `time_t == -1` does—normalize a second discriminator inside the wrapper and document the pair. Recorded in `errno-contract-conventions.md`.

### Tap records need packet-appropriate lifetime, and tap fields should expose normalized consumer semantics

John Thacker's merged !12004 removes RTP's arbitrary static four-entry tap-record pool in favor of distinct `pinfo->pool` allocations for each invocation. His merged !12000 separately makes `info_payload_len` mean actual media payload, excluding RTP padding while retaining enough raw metadata to recover framing. Together they establish that tap contracts need both safe lifetime and clear semantic ownership. Recorded in `tap-data-contract-conventions.md`.

### Shared helpers belong to the layer that owns the abstraction

In !11967, Guy Harris explicitly rejects leaving generic MAC-48 presentation code in the Ethernet dissector: the behavior is not IEEE 802.3-specific, common protocol-tree helpers follow `proto_tree_add_*` naming, and terminology should describe MAC-48 rather than incorrectly imply Ethernet-only semantics. The accepted implementation follows that direction in `epan/proto.c`. Recorded in `shared-helper-ownership-conventions.md`.

## Strong corroborating evidence retained without duplicate notebook rules

!11966 plus !11982/!11983 show that CI state exported for an `after_script` shell must not swallow the main fuzz command's failure; this is already covered more generally in `ci-tooling-conventions.md`. !11979 plus !11986/!11987 reinforce checked arithmetic for signed relative offsets and malicious wire values, already covered by `arithmetic-safety-conventions.md`. !11984/!11989 reinforce timestamp precision as semantic data, already covered by `timestamp-formatting-conventions.md`. !11992 reinforces matching field type/width to the wire representation and including sample captures. !12008 and !11969 are useful reversions showing that a nominally valid tool/refactor change must yield to actual build-matrix or regression-test behavior.

## Frontier check only — not reviewed

!11962, `Fix some more label vs filter issues`, exists in the corpus at commit `ddcaa22b51c68f594e425a23388c3a2086813054` and is merged. It was inspected only to verify that the corpus continues below this batch; it is **not** part of the 50 reviewed MRs recorded above and should remain eligible for the next descending run.