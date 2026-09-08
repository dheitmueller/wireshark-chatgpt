# Automated MR review ledger: !26248–!26297

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Exactly 50 previously unreviewed merge requests were reviewed in this run: **!26248 through !26297 inclusive**. No MR outside that range was counted as reviewed.

## Weighting method

Merged master MRs receive the greatest implementation weight. Stable-branch backports are useful corroboration but normally do not create a second independent architectural rule. Open, abandoned, or superseded implementations receive low implementation weight, while explicit maintainer review comments can still carry substantial evidence about project expectations. Reviewer authority is interpreted using `reviewer-authority.md`, including elevated domain-sensitive weight for Gerald Combs, Guy Harris, Anders Broman, Pascal Quantin, Stig Bjørlykke, Michael Mann, Peter Wu, and Graham Bloice.

## Per-MR accounting

| MR | State / branch | Review depth | Weight and disposition |
|---|---|---|---|
| !26248 | merged / master | discussion + diff | Low-medium. Small RTPS vendor-ID addition. Anders Broman requested fixing CI/commit metadata before merge; useful submission corroboration, no new coding rule. |
| !26249 | merged / master | diff + context | Medium-high. Stig Bjørlykke CVG reassembly work; accepted reassembly implementation, primarily corroborates standard reassembly/state handling. |
| !26250 | merged / master | diff + context | Medium. John Thacker fixes zero-duration iostat behavior so a degenerate valid input still produces one row; edge-case correctness, no separate notebook rule. |
| !26251 | merged / release-4.6 | diff + discussion | Low architectural weight. LBMC checked-add/reassembly hardening backport; corroborates the master fix and existing hostile reassembly-length rules. |
| !26252 | merged / release-4.4 | diff + discussion | Low architectural weight. Same LBMC hardening backport; corroboration only. |
| !26253 | merged / master | targeted diff/context | High. John Thacker prevents LoRaWAN padded-length wrap by using an appropriate width and making decryption failure explicit. Strong integer-width/failure-path evidence; existing notebook rules already cover the general pattern. |
| !26254 | merged / master | targeted diff/context | High. PROFINET secure SXP work merged by Anders Broman; bounds members by declared block lengths and leaves authenticated-encrypted content bounded/opaque where it cannot be decoded. Corroborates containment and honest-opacity parser rules. |
| !26255 | merged / release-4.6 | targeted | Low architectural weight. LoRaWAN overflow backport of !26253. |
| !26256 | merged / release-4.4 | targeted | Low architectural weight. LoRaWAN overflow backport of !26253. |
| !26257 | merged / master | discussion + diff | Very high implementation evidence. Gerald Combs authored/merged pcapng-sysdig length fixes, verifies block existence before option parsing, and adds a test. Corroborates file-parser structural validation and test coverage. |
| !26258 | merged / master | targeted | High authority but protocol-specific. Anders Broman fixes GSM BSSLAP list-loop/container presentation; no new cross-cutting rule. |
| !26259 | merged / master | targeted | Medium-high. Makes `file_compressed.h` include the project header required for `WS_DLL_PUBLIC`, fixing Ubuntu builds. Corroborates self-contained/public-header build rules already recorded. |
| !26260 | merged / master | description + diff | Very high. Removes correctness dependence on Qt connection order by reacting to `modelReset`, after authoritative model state is ready. Promoted to `platform-gui-conventions.md`. |
| !26261 | merged / master | discussion + diff | Medium-high architectural intent, reduced implementation confidence. Extends control-pipe graceful shutdown, but a later unresolved post-merge review comment reports an IN/OUT condition appears swapped and etwdump does not close gracefully. Preserve graceful-control intent; do not treat every code detail as exemplar. |
| !26262 | merged / master | description + diff | Very high. Removes redundant GUI signals that caused refreshes before interface refresh completed. Promoted as part of authoritative-state-transition GUI rule. |
| !26263 | merged / master | targeted | Medium. Correct `%` handling for cross-Windows compiler format checks; portability/build corroboration only. |
| !26264 | merged / master | targeted | Medium. macOS DMG compression/tool update; packaging-specific, no durable general rule extracted. |
| !26265 | merged / master | description + diff | Very high. Routes interface-list notification from the single authoritative manager through `MainApplication`, avoiding unavailable/not-yet-visible window dependencies. Promoted to GUI architecture rule. |
| !26266 | merged / master | metadata + generated diff review | Low. Routine automatic data/translation update; no durable coding convention. |
| !26267 | merged / stable/update branch | metadata + generated diff review | Low. Routine automatic update; no new convention. |
| !26268 | merged / stable/update branch | metadata + generated diff review | Low. Routine automatic update with a failed `manuf` component noted; operational evidence only. |
| !26269 | merged / master | targeted diff/context | High. Replaces potentially billions of malformed-input normalization iterations with arithmetic based on defined C99 division/remainder semantics and adds checked arithmetic. Strong hostile-input performance evidence; consistent with existing parser-edge rules. |
| !26270 | merged / master | description + diff | Very high. Watches preferences only after initial value/start state is known and centralizes default-interface selection in the manager. Promoted to GUI initialization/lifecycle rule. |
| !26271 | merged / master | targeted | Medium-high. Handles applications that never register dissector prefixes before attempting removal; reinforces optional-subsystem lifecycle guarding. |
| !26272 | merged / master | targeted | Medium. Steam protobuf unknown-type/offset fixes from public schema evidence; protocol-specific. |
| !26273 | merged / master | diff + lifecycle context | High. Defensive workaround for dynamically registered extcap preferences missing stashed UI state; explicitly labeled a backportable workaround pending proper fix. Useful lifecycle evidence, but not promoted as the desired architecture. |
| !26274 | merged / master | description + diff | Very high. Removes duplicate PreferencesDialog-to-manager refresh signaling because the manager already consumes the canonical preference-change signal. Promoted with the GUI authoritative-source rule. |
| !26275 | merged / release-4.6 | targeted | Low architectural weight. Netmon malformed-nanosecond hardening backport of !26269. |
| !26276 | merged / release-4.6 | targeted | Low architectural weight. Extcap preference crash workaround backport of !26273. |
| !26277 | merged / release-4.6 | targeted | Low-medium. pcapng-sysdig EVF classification backport; file-format correctness corroboration. |
| !26278 | merged / master | description + diff | Very high. John Thacker change merged by Gerald Combs: extcap preferences affect capture rather than dissection, and “any interface registered a pref” must aggregate across the whole set rather than reflect only the last interface. Strong semantic-state evidence; existing preference architecture is sufficient. |
| !26279 | merged / master | full diff + context | Very high. Parallelizes extcap DLT queries, protects shared hash insertion with a mutex, and documents measured platform-specific startup benefit. Strong concurrency implementation evidence; no broader rule beyond existing synchronization guidance was necessary. |
| !26280 | **open** / master | deep discussion + diff | **Low implementation weight; high review-comment value.** Pascal Quantin caught that a green pipeline was testing an MR diff that had accidentally lost the feature; he and Jaap Keuter also identified unrelated changes, removal of existing formula escaping, forced-on masking, and unresolved usability semantics. Added only the diff-integrity submission rule; masking design is not accepted evidence. |
| !26281 | merged / master | description + tests context | Very high. Follow-up to UDX state machine after libudx team reported three mismatched constants; adds two captures/two tests and aligns timer/SACK limits with the reference implementation. Strongly corroborates differential validation against the actual implementation. |
| !26282 | merged / master | targeted diff/context | High. PKCS#12 decryption now rejects missing algorithm, zero encrypted length, and zero salt instead of accepting misleading crypto-library success on unusable inputs. Corroborates validating semantic preconditions before crypto/helper calls. |
| !26283 | merged / master | targeted | Medium. ORAN subtree presentation fix; protocol/UI-tree specific. |
| !26284 | merged / release-4.4 | targeted | Low architectural weight. Netmon hardening backport of !26269. |
| !26285 | merged / release-4.6 | targeted | Low architectural weight. PKCS#12 validation backport of !26282. |
| !26286 | merged / release-4.4 | targeted | Low architectural weight. PKCS#12 validation backport of !26282. |
| !26287 | merged / master | targeted diff/context | Very high. Uses `tvb_new_child_real_data()` so parent/child lifetime automatically handles exceptions instead of relying on scattered `CATCH` cleanup. Promoted to `memory-lifetime-conventions.md`. |
| !26288 | merged / master | targeted diff/context | High. Ensures XMPP temporary GLists are freed even when dissection throws; corroborates exception-safe resource lifetime rule. |
| !26289 | merged / master | targeted diff/context | High. Checks BER OCTET STRING dissection actually produced a tvbuff before passing it to PBE decryption and adds a NULL-input check. Corroborates explicit helper-output validity contracts. |
| !26290 | merged / master | targeted diff/test context | High. Fixes OOB access in a noisy/debug error path and adds a divide-by-zero test with mismatched operand cardinalities. Reinforces that disabled/log-only paths must still be memory-safe because argument evaluation occurs regardless. |
| !26291 | merged / master | targeted diff/context | High. Validates Toshiba OFFSET line length before reading LEN so stale buffer data from a prior packet cannot be consumed. Strong parser-boundary evidence; existing file-parser rules cover it. |
| !26292 | merged / release-4.6 | targeted | Low architectural weight. AKP validation backport of !26289. |
| !26293 | merged / release-4.6 | targeted | Low architectural weight. DFVM OOB/debug-path fix backport of !26290. |
| !26294 | merged / release-4.4 | targeted | Low architectural weight. DFVM OOB/debug-path fix backport of !26290. |
| !26295 | merged / master | targeted diff/context | High. Bounds CSN.1 recursive arrays by the destination data-member length to prevent writes into adjacent members or outside the structure. Strong memory-boundary evidence; existing length/bounds rules cover it. |
| !26296 | merged / release-4.6 | targeted | Low architectural weight. CSN.1 recursive-array bounds backport of !26295. |
| !26297 | merged / release-4.4 | targeted | Low architectural weight. CSN.1 recursive-array bounds backport of !26295. |

## Durable notebook updates from this batch

- Added `reviewer-authority.md` so future corpus mining gives context-sensitive elevated weight to the core maintainers identified by the user while still preferring current source and merged outcomes.
- Added a submission rule to `submission-conventions.md`: after rebases/force-pushes, inspect the actual current MR diff; a green pipeline validates only what is present in that diff. This comes from Pascal Quantin's review of open !26280, without treating that MR's unfinished masking implementation as accepted architecture.
- Added an authoritative-state-transition rule to `platform-gui-conventions.md` from the merged !26260/!26262/!26265/!26270/!26274 sequence: GUI consumers should react to the manager/model transition that means data is ready, not depend on connection order or duplicate proxy signals; initialization must be distinguished from later change events.
- Added an exception-safe tvbuff ownership rule to `memory-lifetime-conventions.md` from merged !26287, corroborated by !26288: express ownership structurally (for example parent/child tvbuff relationships) so exceptions do not require every catch path to remember cleanup.

## Not promoted as new rules

The LBMC, LoRaWAN, netmon, PKCS#12/AKP, DFVM, Toshiba, and CSN.1 hardening changes are strong evidence, but their general lessons are already represented in the notebook's checked-arithmetic, bounds, parser-edge, nullability, and lifetime guidance. The stable-branch copies were therefore retained as corroboration instead of duplicating those rules.

!26261's graceful-control architecture remains useful, but its later unresolved post-merge bug report prevents treating the exact implementation as a pristine exemplar. !26280 remains open with unresolved review threads, so its feature design was deliberately not promoted.
