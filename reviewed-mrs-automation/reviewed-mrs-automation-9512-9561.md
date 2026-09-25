# Automated Wireshark MR review ledger: !9512–!9561

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: newest available previously-unreviewed MRs toward older MRs.

## Selection and duplicate avoidance

Before selecting this batch, the already-reviewed set was reconciled against the review tracking available on the latest accumulated notebook review state, including `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, the immediately preceding exact ledger `reviewed-mrs-automation-9562-9612-gap9604.md`, and the available historical per-run tracking. Candidate membership was checked by exact MR number rather than by assuming that a numeric interval was fully reviewed.

The historical `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` ledger was re-opened and still contains exactly 50 unique MRs, !17571 through !17620 inclusive.

The previous run's inspection of !9561 was only a frontier probe and was not counted as a review. Exact tracking searches found no prior review entry for any candidate in this batch. The fifty highest-numbered corpus MRs absent from the reconstructed reviewed set are therefore exactly !9561 through !9512 inclusive.

Exactly reviewed in this run (50 unique MRs, descending):

!9561, !9560, !9559, !9558, !9557, !9556, !9555, !9554, !9553, !9552,
!9551, !9550, !9549, !9548, !9547, !9546, !9545, !9544, !9543, !9542,
!9541, !9540, !9539, !9538, !9537, !9536, !9535, !9534, !9533, !9532,
!9531, !9530, !9529, !9528, !9527, !9526, !9525, !9524, !9523, !9522,
!9521, !9520, !9519, !9518, !9517, !9516, !9515, !9514, !9513, !9512

Count: **50**. Maximum: **!9561**. Minimum: **!9512**.

Status mix: **49 merged, 1 closed/unmerged (!9526)**. Merged master changes were weighted most heavily. !9526 was down-weighted as superseded. Although !9512 itself merged, its implementation was immediately reverted by merged !9516 after post-merge review exposed an invalid field registration/build breakage; it is therefore treated as negative evidence rather than an accepted implementation exemplar.

## Per-MR review notes

| MR | Outcome / depth | Review note |
|---|---|---|
| !9561 | merged / Scanned | AUTHORS formatting cleanup only; no new durable convention. |
| !9560 | merged / Scanned | Qt About-dialog capitalization cleanup; presentation-only. |
| !9559 | merged / Discussion-focused | Moves the Unix personal extcap directory from configuration storage to the architecture-dependent personal library area. Gerald Combs explicitly requested a release-note entry, which the accepted MR added. Corroborates existing user-visible-change/release-note guidance. |
| !9558 | merged / Scanned | NSIS uninstall compatibility cleanup; no broader convention extracted. |
| !9557 | merged / Discussion-focused | RTPS instance-state support. Alexis La Goutte caught trailing whitespace/static-analysis findings; Gerald Combs identified an inbound macOS fix that required rebasing. Useful ordinary pre-submit/CI corroboration, but no new architecture rule promoted. |
| !9556 | merged / Scanned | GTPv2 Inter-RAT session tracking; protocol-specific state extension, no additional durable review guidance. |
| !9555 | merged / Scanned | AUTHORS generation maintenance; build/documentation plumbing. |
| !9554 | merged / **Deep / promoted** | USBLL invalidates endpoint reassembly/retransmission state on a protocol SET ADDRESS boundary while deliberately preserving default-address bootstrap state. Promoted as a logical-reset/state-lifetime rule. |
| !9553 | merged / Scanned | Adds a wsutil documentation-path helper; filesystem refactor, no separate convention. |
| !9552 | merged / Deep / corroboration | Martin Mathieson's typed-item checker run fixes numerous real field-width, item-length, duplicate-filter and mask issues. Checker exceptions for reserved/unknown non-contiguous fields are widened narrowly and case-insensitively; corroborates existing checker guidance without weakening width/type checks. |
| !9551 | merged / Scanned | Stable H.261 vmvd encoding fix corresponding to the master correction; no lesson beyond !9519. |
| !9550 | merged / **Deep / promoted** | John Thacker makes pcapng NRB contents persistent Wiretap block data and replays already-read name-resolution metadata when redissection installs fresh callbacks. Promoted as a capture-metadata replay/redissection rule. |
| !9549 | merged / Scanned | Metamako naming/timestamp wording cleanup; no broader review lesson. |
| !9548 | merged / Scanned | Qt packet selection/current-index correction; one maintained-branch instance of the !9546–!9548 sequence. |
| !9547 | merged / Scanned | Qt packet selection/current-index correction; maintained-branch duplicate/corroboration. |
| !9546 | merged / Scanned | Qt packet selection/current-index correction; accepted master/family implementation. |
| !9545 | merged / **Deep / promoted** | MSYS2 CI now passes the workflow's exact `github.sha` into PKGBUILD and checks out/describes that commit rather than implicitly building moving master. Promoted as a CI source-reproducibility rule. |
| !9544 | merged / Scanned | John Thacker adds request/response semantics to GTP FT_FRAMENUM fields so packet-relation arrows work; useful typed-field metadata example, but narrow. |
| !9543 | merged / Scanned | WiX stale component cleanup. |
| !9542 | merged / Discussion-focused | CIP connection-analysis expansion. Gilbert Ramirez caught use of a generated item outside the branch where that item exists; fixed before merge. Useful local control-flow review evidence, not a new general rule. |
| !9541 | merged / Discussion-focused | Embeds the generated Qt authors list as a resource after discussion of binary size, startup cost and packaging/documentation semantics. Accepted compromise is GUI/resource-specific. |
| !9540 | merged / Scanned | Follow Stream refactor removing another fixed follow-type dependency; corroborates the later registered-follower architecture already captured elsewhere. |
| !9539 | merged / Scanned | Ships license/acknowledgements as Qt resources; packaging/UI-only. |
| !9538 | merged / Deep / corrective evidence | Deduplicates dependent-frame entries and clears derived dependencies on reset, but linear-list membership made a real large capture effectively unusable. Already-reviewed !9622 replaced the set-like list with a hash table and removed the CPU problem. Strong corroboration that semantic sets with large membership require appropriate set/hash structures. |
| !9537 | merged / Scanned | Installs CSS under DOCDIR; packaging-path cleanup. |
| !9536 | merged / Scanned | Stable-branch compiler-version check update; no new rule beyond build-baseline guidance. |
| !9535 | merged / Scanned | Stable-branch compiler-version check update; duplicate/corroboration. |
| !9534 | merged / Scanned | HTTP/2 SETTINGS decoding now tolerates a missing session object and confines session mutations to the stateful path. Straightforward defensive fix. |
| !9533 | merged / Scanned | Moves PDML documentation resources to DOCDIR; packaging/filesystem maintenance. |
| !9532 | merged / Scanned | ENIP CPF framing only applies to List responses; protocol-specific malformed-packet false-positive fix. |
| !9531 | merged / Scanned | Follow Stream removes an unnecessary TCP-type check because fragment storage itself indicates applicability; ongoing registered-follower cleanup. |
| !9530 | merged / Discussion-focused | TDS SSPI token parsing added with a supplied pcap after Alexis requested one. John Thacker later questioned the non-NTLM dispatch based on MS-TDS/SPNEGO semantics and asked for evidence that generic GSSAPI was needed. Retained as post-merge review evidence, not promoted as an accepted general dispatch pattern. |
| !9529 | merged / Scanned | Compiler-version check adjustment required by an earlier baseline change; no new rule. |
| !9528 | merged / Scanned | WiX stale dftest reference removal. |
| !9527 | merged / **Deep / promoted** | John Thacker moves DHCP time formatting from one-off tree-item formatting into registered `BASE_CUSTOM`/formatter metadata so the same representation is available to custom columns and other field consumers. Promoted as a field-presentation rule. |
| !9526 | closed/unmerged / Down-weighted | Proposed global tshark `--machine-readable` conversation-statistics option. Closed after Michael Mann noted the functionality was handled by !19329. Not an implementation exemplar. |
| !9525 | merged / Scanned | RPM documentation-file cleanup. |
| !9524 | merged / Scanned | Falco personal-plugin scanning support; component-specific. |
| !9523 | merged / Scanned | Logray AppImage packaging support; no reusable review convention. |
| !9522 | merged / Scanned | 3.6 release-version maintenance. |
| !9521 | merged / Scanned | 4.0 release-version maintenance. |
| !9520 | merged / Discussion-focused | SMB2 lock sequence fields. A reviewer caught the display label for the index field after merge; Alexis pointed to corrective !9588. Useful reminder to audit sibling field labels, but no new convention needed. |
| !9519 | merged / Discussion-focused | H.261 vmvd encoding fix. Martin Mathieson asked for `ENC_NA` consistency on the one-byte field; corrected. Corroborates existing field-decoding encoding guidance. |
| !9518 | merged / Scanned | 3.6 release build maintenance. |
| !9517 | merged / Scanned | 4.0 release build maintenance. |
| !9516 | merged / **Deep / promoted negative evidence** | Immediate revert of !9512 after it broke Wireshark. Martin Mathieson explicitly thanked the revert for unbreaking the build. The pair is used to capture both field-registration semantics and CI-before-merge discipline. |
| !9515 | merged / Scanned | NFS IO_ADVISE tree/mask display correction; protocol-specific. |
| !9514 | merged / Discussion-focused | Removes developer-oriented dftest from shipped installers/manpage. Discussion distinguishes developer utilities from supported installed-user tools and suggests tshark as the better long-term filter-validation surface. Packaging-specific. |
| !9513 | merged / Scanned | Installs HTML manuals under DOCDIR and updates help-path lookup; filesystem/packaging maintenance. |
| !9512 | merged then reverted / **Deep / negative evidence** | Added a synthetic grouping field as `FT_STRING` with a nonzero mask and merged before a pipeline passed. John Thacker noted that FT_STRING/FT_NONE cannot carry that bitmask and that an always-empty grouping field should be `FT_NONE`; Lars Völker reported the breakage. Reverted by !9516 within hours. |

## Durable findings promoted

- Capture metadata that rebuilds higher-level state must be retained independently of the original sequential read and replayed when redissection installs a fresh consumer/callback; !9550 is a John Thacker-authored merged exemplar for NRBs.
- CI/package recipes that clone Wireshark independently must build the exact commit under test, not a moving branch tip; !9545 passes the workflow SHA into the MSYS2 package source selector.
- If a protocol event ends the identity/lifetime of reassembly or retransmission state, invalidate that state at the protocol boundary; preserve only bootstrap facts the protocol explicitly keeps valid (!9554).
- Field formatting intended to be part of the field's presentation contract should live in registered field metadata (`BASE_CUSTOM`/formatter) instead of a one-off `proto_tree_add_*_format_value()` call, so columns and other consumers receive it too (!9527).
- Synthetic grouping-only tree fields should represent “no value” as `FT_NONE` rather than a fake empty string, and string/none fields must not carry integer bit masks. !9512 is negative evidence; !9516 is the accepted correction/revert, with direct John Thacker review.
- Do not treat “merged” as proof that a patch was validated when it was merged before CI. !9512 was merged before any pipeline passed, immediately broke the build, and was reverted by !9516. Wait for applicable required checks before accepting a change.
- !9538 plus the already-reviewed corrective !9622 reinforces that data with set semantics and potentially large cardinality should use a set/hash representation rather than repeated linear membership scans.

## Frontier

The corpus is not exhausted. `mr_9511.json` exists at the same corpus commit; it is merged and titled `RPM: Fix header installation`. It was inspected only as the next-frontier probe and is **not** counted as reviewed in this run.
