# Wireshark MR Review Automation: !23509–!23558

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from all available review tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking, and the individual ledgers under `reviewed-mrs-automation/`. Individual MR entries were used rather than assuming that a numeric interval was reviewed from a filename or neighboring batch. The historical !17571–!17620 batch remains explicitly preserved and counts as reviewed. Searches of the tracking for the candidate endpoints and 235xx entries found no prior review of the selected MRs.

Exactly 50 MRs were reviewed, in descending order. The exact set is:

!23558, !23557, !23556, !23555, !23554, !23553, !23552, !23551, !23550, !23549,
!23548, !23547, !23546, !23545, !23544, !23543, !23542, !23541, !23540, !23539,
!23538, !23537, !23536, !23535, !23534, !23533, !23532, !23531, !23530, !23529,
!23528, !23527, !23526, !23525, !23524, !23523, !23522, !23521, !23520, !23519,
!23518, !23517, !23516, !23515, !23514, !23513, !23512, !23511, !23510, !23509.

## Review weighting and findings

Merged master work and substantive maintainer reasoning were weighted most heavily. Stable backports were used mainly as corroboration. Closed probes, temporary diagnostics, and superseded work were retained in the exact reviewed set but down-weighted as engineering evidence.

- **!23550 — Deep/promoted, merged master.** Gerald Combs corrected the proposed product assignment for `etwdump`: despite using ETW, it emits network packets and therefore stays with Wireshark while event-oriented extcaps move toward Stratoshark. Promoted to `extcap-product-boundary-conventions.md`.
- **!23538 — Deep/promoted, merged release-4.4.** Gerald Combs restores the exported `ws_base32_decode()` declaration and a compatibility stub after its removal broke the stable ABI; John Thacker approved. Promoted to `abi-compatibility-conventions.md`.
- **!23509 and !23525 — Deep/promoted, merged master plus backport.** GSM SIM APDU command/response text now appends to existing `COL_INFO` with its own separator instead of overwriting caller context or requiring the caller to insert punctuation. Promoted to `packet-column-conventions.md`.
- **!23540 — Deep/corroboration, merged master.** GitLab merged-results pipelines must validate the source commit (`CI_MERGE_REQUEST_SOURCE_BRANCH_SHA`) rather than the temporary synthetic merge commit. This is already captured in `ci-source-identity-conventions.md` from later merged evidence, so no duplicate rule was added.
- **!23557 — Discussion-focused/corroboration, merged.** Gerald Combs caught an infinite loop introduced during an offset/length API conversion. Strong evidence that mechanical signed/unsigned and TVB-search migrations require re-checking loop progress and termination; existing parser-progress/type-domain guidance already covers it.
- **!23548, !23553 and !23552 — Deep/corroboration, merged master/backports.** SOCKS proxy state is tied to the specific outer connection instance so consecutive sessions reusing a 5-tuple do not inherit stale inner conversation state. Existing state-identity/conversation guidance covers the principle.
- **!23551 — Deep/corroboration, merged.** SOCKS Decode-As behavior uses the actual matched selector rather than a fixed protocol port. Already captured from !23597 in `dissector-dispatch-context-conventions.md`.
- **!23543 — Discussion-focused, merged.** Gerald Combs required the corresponding Stratoshark MainWindow accessibility changes when parallel Wireshark UI behavior changed. Useful cross-product review evidence, but one focused UI change was not generalized into a new rule.
- **!23539, !23521 and !23520 — Portability corroboration, merged.** Avoid nonportable `uint`, guard fallback platform constants when system headers may define them, and account for 32-bit signedness differences. These reinforce existing portability/type guidance; no duplicate rule was added.
- **!23536 — Down-weighted, closed.** Martin Mathieson noted that the proposed direct edit targeted a generated dissector and must instead be made in the ASN.1 template/configuration and regenerated. Strong review guidance, but generated-source ownership is already an established notebook convention and the MR itself was not merged.
- **!23531, !23528, !23523 and !23522 — Down-weighted diagnostic/probe sequence.** Guy Harris temporarily added and then removed validate-commit debugging while isolating GitLab merge-result commit identity. The temporary instrumentation and deliberately long-branch test MRs were not promoted; the durable accepted result is represented by merged !23540.
- **!23529 and !23534 — Corroboration, merged.** Continue the project pattern of using proto-tree return-value helpers to avoid fetching packet values twice and of reusing existing protocol helpers rather than reproducing parsing logic.
- **!23524 — Scanned, merged backport.** Builds a composite TVB so the called GSM SIM dissector receives the complete response representation it expects. Useful example of adapting data to a subdissector contract, but no new convention beyond existing dissector-API/subset-TVBuff guidance.
- **!23512 and !23518 — Discussion-focused/corroboration, merged master/backport.** NULL-check ALPN before string comparison; John Thacker noted `g_strcmp0()` as an existing null-safe helper. Straightforward defensive API use rather than a new project-wide rule.
- **!23511 — High-authority corroboration, merged backport authored/merged by Guy Harris.** BLF writer interface mappings expand when a new IDB is supplied, reinforcing that writer-side derived mappings must track dynamically growing interface metadata. The case was too format-specific to promote separately.
- **!23558, !23547, !23544/!23545/!23546, !23530 and !23513 — Corroboration, merged.** TVB search migrations, zero-length/progress handling, bounds checks, and unsigned offset/length cleanup are covered by existing parsing and arithmetic guidance.
- **!23556/!23555/!23554 — Scanned, merged.** Automatic generated-data updates; no durable engineering rule.
- **!23549, !23537, !23535, !23533, !23532, !23527, !23526, !23519, !23517, !23516, !23515, !23514 and !23510 — Scanned, merged.** Build/tooling or protocol-local correctness changes with no additional general convention beyond existing notebook guidance.

## Notebook changes

Promoted in this run:

- `abi-compatibility-conventions.md`
- `extcap-product-boundary-conventions.md`
- `packet-column-conventions.md`

The existing CI source-identity, parser-progress, conversation/state, Decode-As dispatch, generated-source, helper-reuse, portability, and defensive-parsing conventions were intentionally not duplicated.