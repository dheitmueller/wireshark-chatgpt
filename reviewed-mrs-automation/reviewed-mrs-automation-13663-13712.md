# Wireshark MR review run: !13712 through !13663

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `776800b7be78fdd75bb508604f40390b624d498e`

Selection was reconciled against all available review tracking in `reviewed-mrs.md`, the aggregate automation tracking, and the per-run ledger inventory under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains part of the already-reviewed set. The preceding exact ledger covered !13762 through !13713 and explicitly treated !13712 only as a frontier probe, not as reviewed. The corpus remained at the same commit during this run. Exact MR-number set subtraction therefore selected the fifty highest-numbered previously unreviewed corpus MRs listed below; no numeric range was assumed reviewed merely from partial ledger coverage.

## Exact reviewed set

!13712, !13711, !13710, !13709, !13708, !13707, !13706, !13705, !13704, !13703,
!13702, !13701, !13700, !13699, !13698, !13697, !13696, !13695, !13694, !13693,
!13692, !13691, !13690, !13689, !13688, !13687, !13686, !13685, !13684, !13683,
!13682, !13681, !13680, !13679, !13678, !13677, !13676, !13675, !13674, !13673,
!13672, !13671, !13670, !13669, !13668, !13667, !13666, !13665, !13664, !13663.

Exactly 50 MRs were reviewed. **47 were merged and three were closed/unmerged: !13712, !13693, and !13688.** Closed/superseded work was down-weighted except where its discussion or test artifacts clarify the accepted implementation. !13693 was superseded by merged !13702. Master changes were weighted more heavily than stable cherry-picks, and substantive maintainer feedback was weighted more heavily than mechanical approvals.

## Durable findings promoted

- **!13697 — nonblocking connect readiness is completion, not success.** John Thacker's merged master `androiddump` fix replaces long blocking UNIX loopback connects with proper nonblocking handling. Guy Harris gave especially authoritative review by pointing to the POSIX asynchronous-connect contract. John then documented the key portability distinction: after UNIX `select()` reports writability, code must query `getsockopt(SOL_SOCKET, SO_ERROR)` because readiness can mean either successful or failed completion, whereas the existing Windows path classified failures differently. New `socket-io-conventions.md` records the `SO_ERROR` completion rule and platform-semantics testing guidance.

- **!13702, with superseded !13693 — established protocol identity can outlive the transport tuple.** The merged QUIC migration/NAT-rebinding fix associates an already identified QUIC connection with the newly observed conversation so reverse-direction packets to the new address can find and decrypt against the same state, including zero-length connection-ID cases. John Thacker's review distinguished forbidden active-migration CID reuse from legitimate NAT rebinding. The superseded predecessor supplied a concrete migration capture, keylog, and packet-level expectations, which remain valuable regression-test evidence. `conversation-identity-conventions.md` now records the transport-location-versus-protocol-identity rule and bidirectional migration testing practice.

- **!13680 — use structured serialization rather than delimiter parsing for extensible machine output.** John Thacker's merged capture change replaces the tab/newline machine-readable interface list with JSON because interface names/descriptions can contain tabs and because the schema was expected to grow toward combined capability/statistics discovery. The reader validates malformed JSON as an IPC error instead of silently accepting partially split records. `subprocess-ipc-lifecycle-conventions.md` now records the serialization and validation boundary rule.

- **!13670, corroborated by stable !13674/!13675/!13676 — remove unwanted Qt connections at their declarative source.** The graph dialogs had a `.ui` `accepted()`→`accept()` connection that C++ attempted to disconnect later. Qt 6 ordering made that pattern unreliable and could destroy the dialog while a nested FileDialog remained open. The accepted master fix removes the connection from the `.ui` files rather than creating it and depending on constructor ordering to cancel it. `platform-gui-conventions.md` now records the declarative connection/lifecycle rule.

- **!13665 — semantic analysis flags must form a coherent classification.** During the TCP duplicate-ACK/window-update correction, John Thacker explicitly argued that if a packet is a duplicate ACK *rather than* a Window Update, the analyzer should clear the superseded Window Update flag instead of adding another flag and compensating in color filters. He also caught redundant flag/filter logic. New `analysis-classification-conventions.md` records that mutually exclusive analysis interpretations should be normalized at the analyzer that owns them, with tests asserting absence as well as presence of flags.

- **!13705 and !13671 — downstream package transitions and relocatable metadata need upgrade/layout testing.** Debian maintainer Balint Reczey explained on merged !13705 that moving files between binary packages requires versioned `Breaks` + `Replaces` or upgrades can stop on file overlap; Gerald Combs explicitly gave that downstream packaging contribution project-level weight. Merged !13671 later received concrete evidence that a fixed relative `pcfiledir` depth broke Debian multiarch and an external libvirt plugin. New `packaging-conventions.md` records both package-ownership transition testing and multiarch/external-consumer validation for relocatable metadata.

## Strong corroborating and review findings retained without duplicating notebook rules

- **!13708** removes SSH's fixed Gerrit port 29418 registration because that port is not an IANA-assigned SSH service and can collide with ordinary ephemeral traffic. This strongly corroborates the existing dissector-table semantic-ownership rule: protocol popularity/convention is not sufficient to claim a fixed selector globally.
- **!13711** fixes an extcap Coverity issue, and Jaap Keuter then caught a no-libpcap link failure in the first revision. The follow-up feature guard reinforces building optional-feature-off configurations, not merely the maintainer's normal dependency-rich build.
- **!13678** is a useful stable-branch policy example: the proposed backport was not needed because the regression-causing change had never been backported to that branch. This corroborates evaluating the target branch's actual ancestry before backporting a fix.
- **!13679** contains substantial Guy Harris platform-loader discussion while changing plugin layout/versioning. Guy noted that loader semantics (`dlopen()` and accepted file names) are not the same thing as Finder/UI naming conventions. The broader plugin redesign area had later contested/reverted work, so this evidence is retained without promoting a new hard plugin-architecture rule.
- **!13688** was closed after Gerald Combs objected to dropping extcap manual pages merely because extcap binaries are not normally on the user's shell PATH; the manuals still provide useful CLI options/examples. As unmerged negative evidence it is not treated as a project-wide installation rule.
- **!13664** and stable **!13686** handle GCC `-Wclobbered` around Wireshark's longjmp/exception machinery by using `volatile`; John Thacker noted longstanding GCC warning/optimization behavior. Useful compiler/exception evidence, but narrower than existing portability/static-analysis guidance.
- **!13682/!13683/!13684/!13685** update Clang CI to version 17 across master and supported release branches. These are maintenance/corroboration for keeping supported toolchain validation aligned, not an independent convention.
- **!13695** changes `passed_dfilter` initialization so frames are included by default and dissectors that intentionally hide internal events opt out. This is a useful default-state design example but too subsystem-specific to promote here.
- **!13671** also demonstrates why a merged portability improvement can still need later downstream correction: successful project CI did not exercise Debian's multiarch `.pc` placement or the external plugin consumer that exposed the bad relative prefix.
- **!13663** adds SRT handshake extension dissection and display corrections after iterative Alexis La Goutte review. The feedback was largely protocol/style specific and did not add a stronger cross-project rule.

The remaining MRs were protocol-specific dissector fixes/features, stable backports, dependency/toolchain refreshes, generated-data/documentation maintenance, and focused UI/build corrections that did not establish a stronger durable convention than the notebook already contains.

## Notebook commits from this run

- `7b161be5c95c5b732bf958f1ba134320ffb73a8a` — add POSIX nonblocking-connect/`SO_ERROR` socket completion semantics from !13697.
- `8b78df4e074790a8c45dc733698223c3bed5e1e0` — add structured machine-readable subprocess serialization and malformed-output validation from !13680.
- `f9fed8a3c198df2d78a808bf1d99af868177eafc` — add QUIC migration/rebinding conversation-association and regression-testing guidance from !13702/!13693.
- `53a1e401e9ca8d0a9120eaeade0417f462f357db` — add declarative Qt signal/lifecycle guidance from !13670 and stable backports.
- `531da098f6629d30f2048015609f85f8d0526090` — add coherent/mutually-exclusive analysis classification rule from !13665.
- `36a2ef781dd3a073ff41d69a723d9facf26271b8` — add Debian package ownership-transition and relocatable pkg-config/multiarch validation rules from !13705 and !13671.

## Frontier

`mr_13662.json` exists in corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054` and is merged. It was inspected only to verify that the corpus continues and was **not** counted as reviewed. Therefore the corpus is not exhausted; absent newly scraped higher-numbered unreviewed material, !13662 is the next descending candidate.