# Wireshark MR review ledger: !13312 through !13263

## Run metadata

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook repository: `dheitmueller/wireshark-chatgpt`
- Notebook base before this run: `627aa0d88b66039cd3fd544764b76030677fad65`
- Direction: descending from the newest available previously unreviewed MR.
- Review limit: exactly 50 MRs.

Before selecting this batch, the available review tracking on notebook `main` was reconciled, including `reviewed-mrs.md`, the per-run ledgers under `reviewed-mrs-automation/`, and searches for the candidate MR numbers. The historical !17571-!17620 batch remains explicitly counted as reviewed. Selection was based on explicit MR membership, not on assuming numeric ranges were complete.

A prior chat completion had claimed that !13312-!13263 had already been reviewed, but the repository did not contain the claimed `reviewed-mrs-automation-13263-13312.md` ledger or corresponding tracking entries. The preceding real ledger (`reviewed-mrs-automation-13313-13362.md`) recorded !13312 only as a frontier probe and explicitly did not count it as reviewed. This run therefore performs and records the authoritative review of these 50 MRs.

## Exact reviewed MR set

The exact MRs counted as reviewed in this run are:

`!13312, !13311, !13310, !13309, !13308, !13307, !13306, !13305, !13304, !13303, !13302, !13301, !13300, !13299, !13298, !13297, !13296, !13295, !13294, !13293, !13292, !13291, !13290, !13289, !13288, !13287, !13286, !13285, !13284, !13283, !13282, !13281, !13280, !13279, !13278, !13277, !13276, !13275, !13274, !13273, !13272, !13271, !13270, !13269, !13268, !13267, !13266, !13265, !13264, !13263`

Status summary: 49 merged; 1 closed/unmerged (`!13263`). The closed MR was reviewed but down-weighted as implementation evidence.

## Durable findings promoted to the notebook

### Startup configuration precedence

Merged master !13285, authored and merged by John Thacker, defines an explicit startup ordering between configuration-source selectors, preference loading, command-line preference overrides, projection of preferences into capture options, and final invocation-specific capture-option overrides. Persisted preferences are defaults; explicit invocation options must not be overwritten by a later lower-precedence projection. Merged !13300 carries the same fix to release-4.2.

Notebook: `configuration-precedence-conventions.md`

Commit: `0e1660882c598cf5922338372474b5d4df264783`

### Order-independent request/response transaction state

Merged master !13289, authored and merged by John Thacker, keys RSVD task state by request ID, performs lookup-before-create, permits response-side state to exist before a request has been observed, and fills missing request-side information later. Persistent state is established only at the intended first-pass points and reused on redissection.

Notebook: `request-response-state-conventions.md`

Commit: `d748446a066a3f229f4f1b9eb9ccca9e81b4e3c6`

### Implicit protocol-version inference from normative structure

Merged master !13293 uses a version-dependent, specification-defined RSVD response structure/length to distinguish protocol layouts where no explicit version marker is carried. The durable rule is to infer variants from an independent normative structural invariant rather than from an incidental value whose interpretation already depends on the chosen version.

Notebook: `protocol-version-inference-conventions.md`

Commit: `a174e9a66723d48d3684edb31ce2131391c886ab`

### Parent-aware Qt model notifications

Merged master !13264 fixes the Advanced Preferences model by constructing the `dataChanged()` range with the edited item's parent and by notifying all columns whose presentation can change. Stable backports !13265, !13266, and !13267 carry the same correction.

Notebook: `qt-model-notification-conventions.md`

Commit: `1698c54fa2adf0ce59de4877560bee5e187244e7`

### Capture-host checksum-offload artifacts

Merged release-4.2 !13272, backporting the corresponding master change, recognizes the well-defined partial pseudo-header checksum left by local checksum offload rather than reporting it as an ordinary bad checksum. Merged documentation !13275, with detailed Peter Wu review, clarifies that this artifact is expected on locally generated/transmitted packets at the capture point. The durable rule is to recognize a specific documented offload signature without weakening genuine corruption diagnostics.

Notebook: `capture-offload-conventions.md`

Initial commit: `6c6648163c37e99b1a6334d09a256c802d7418ba`

Clarification commit: `d3724a4e1eb9c5318069caffa136a62822a9bb65`

## Strong corroboration retained without duplicate notebook rules

- !13312 is a Guy Harris-authored-and-merged BLF comment/documentation improvement. High-authority evidence, but it adds explanatory source comments rather than a new cross-cutting convention.
- !13311 reinforces the existing recent-state/history rule: persisted MRU order must preserve semantic newest-first behavior and retention limits must not accidentally freeze an old subset.
- !13310 reinforces the existing cleanup-precondition rule; later reviewed !13362 demonstrates why cleanup must be conditioned on successful resource initialization rather than merely on variable scope.
- !13294 reinforces the existing dissector length-semantics rule. SCSI responses can legitimately be shorter because the request's Allocation Length is an upper bound; request-side limits must be carried into response dissection rather than treating every short response as malformed/capture truncation.
- !13291 reinforces parser context sensitivity: an identifier-shaped token that does not resolve as a field may still be a literal where grammar permits literals, while field-only operators must reject it.
- !13288/!13298 and !13297 repair XML prolog regular-expression bounds/greediness; useful parser evidence but already covered by bounded parsing principles.
- !13277 reinforces ABI/package metadata accuracy: Debian symbol files must name the actual exported symbols and record the correct released introduction version, not an RC/development version.
- !13276 is useful protocol-dispatch evidence: MongoDB can use plaintext or TLS on the same TCP port, so port number alone cannot force a single security-layer interpretation; the accepted dissector combines plaintext structural heuristics with TLS registration.
- !13270/!13274 correct checksum presentation to use zero-padded four-digit hexadecimal formatting.
- !13269 and !13284/!13287/!13296 plus !13301-!13308 provide checker-driven field/item naming and registration hygiene evidence, but do not add a new convention beyond existing source-checker and field-registration guidance.
- !13292 adds native MPEG-2 TS Wiretap writing with explicit format constraints; protocol/file-format-specific implementation, no new cross-cutting rule promoted.
- !13283-!13281 and !13280-!13278 are release/version/build preparation changes and were reviewed at lower depth.

## Down-weighted unmerged MR

- !13263 was a release-4.0 DHCPv6 option-79 backport and was closed rather than merged. Review discussion treated it as an enhancement rather than an appropriate bug-fix backport with 4.2 imminent. It is counted as reviewed but not used as accepted implementation guidance.

## Frontier

`!13262` exists at the same corpus commit and is merged. It was fetched only to verify the next frontier and is **not** counted as reviewed in this run. Absent newly scraped higher-numbered unreviewed MRs, !13262 is the next descending candidate.

The corpus is not exhausted, so no scraper-restart notification is required for this run.