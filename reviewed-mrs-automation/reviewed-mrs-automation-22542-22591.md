# Wireshark MR review automation ledger: !22591–!22542

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: descending MR number, newest available previously-unreviewed MRs first.

## Exact reviewed set

This run reviewed exactly 50 MRs:

!22591, !22590, !22589, !22588, !22587, !22586, !22585, !22584, !22583, !22582,
!22581, !22580, !22579, !22578, !22577, !22576, !22575, !22574, !22573, !22572,
!22571, !22570, !22569, !22568, !22567, !22566, !22565, !22564, !22563, !22562,
!22561, !22560, !22559, !22558, !22557, !22556, !22555, !22554, !22553, !22552,
!22551, !22550, !22549, !22548, !22547, !22546, !22545, !22544, !22543, !22542.

No numeric-range assumption was used to establish prior review state. The available per-run ledgers and `reviewed-mrs.md` were consulted first; !22592 was the highest already-reviewed frontier in this part of the corpus and no individually tracked reviewed MR below it displaced an entry in this batch. Corpus presence was then checked for the selected range.

The historical !17571–!17620 review batch remains part of the already-reviewed set and is preserved/counts as 50 previously reviewed MRs.

## Review conclusions

### Promoted to notebook

- **!22586 — Add `extern "C"` for C++ compatibility — merged.** Public C headers must provide their own correctly scoped C++ linkage guards. Consumer-side wrapping of an entire include is not an adequate substitute because transitively included headers may contain C++ constructs that cannot appear inside `extern "C"`. John Thacker approved and merged the fix. Added to `public-header-validation-conventions.md`.
- **!22579 — CMake+tools: Automatically generate some dissector/public-header lists — merged.** Installed public-header inventory should be generated from explicit source-level public-interface evidence, while retaining an explicit opt-in for public structure/tap/plugin headers that contain no exported functions. Michael Mann explicitly raised the third-party-plugin compatibility case during review; the final design includes a `PUBLIC_HEADER` escape hatch and was merged by John Thacker. Added to `public-header-validation-conventions.md`.

### Strong corroboration of existing notebook guidance

- **!22591, !22577, !22575, !22574, !22572, !22567, !22564, !22561, !22545, !22543** are part of the pointer-sign/type-domain cleanup series. They reinforce that pointer/result types must match the actual semantic and generated-code contracts rather than being silenced with casts; the ASN.1 cases in particular distinguish signed INTEGER/CHOICE results from inherently non-negative ENUMERATED/tag/constrained results.
- **!22589** is the master-side PKCS#12 hash-iteration resource-limit fix whose release backports were already encountered in later-numbered review batches. It strongly corroborates the existing input-resource-limit convention: syntactically valid packet values still require defensible implementation limits when they can directly request expensive computation.
- **!22587** reinforces ownership/lifetime discipline: callback data with shared cleanup paths must have one clear owner and be freed exactly once.
- **!22583** reinforces uniqueness of per-packet/proto-data keys when multiple logically distinct private-data objects are stored under the same protocol.
- **!22558** reinforces wiretap/file-format validation: for an uncompressed container, validate that the stored segment length agrees with the declared/expected uncompressed length before consuming it.
- **!22554/!22551/!22555** reinforce length-before-parse handling for text protocols: validate that a line is long enough to contain a grammar-required token before advancing into or parsing that token.
- **!22553/!22552/!22549** reinforce progress and sentinel handling in parser/search loops: a `-1` not-found result must be normalized to a terminating boundary rather than fed back into offset arithmetic where it can move the cursor backwards or create an infinite loop.
- **!22550** reinforces conservative heuristic classification: TPKT's already-weak continuation heuristic treats an impossible header length as continuation data instead of proceeding as though a valid TPKT header had been established.
- **!22546** reinforces using TVBuff APIs instead of pulling raw pointers out of TVBuffs and manually recreating bounds/string operations; this both preserves buffer semantics and avoids needless signedness conversions.
- **!22544** reinforces explicit fragmented-message state and expert reporting when fragments are missing; TEAP fragmentation cannot assume the total Message Length appears on every fragment.
- **!22548/!22562** reinforce the application-layer-boundary pattern already recorded in the notebook: application-specific GUI/command-line policy belongs in application-specific subclasses/data/callbacks rather than repeated runtime checks inside a shared base layer.
- **!22566/!22563/!22560/!22559/!22557** reinforce treating duplicate expert-info display-filter names as a correctness problem and using tooling to make the invariant executable.
- **!22571** reinforces reproducible/generated-data workflow conventions by updating both the generated BACnet vendor table and the script/path ergonomics used to regenerate it.

### Reviewed but not promoted as new durable rules

- **!22590** is mechanical funnel whitespace cleanup.
- **!22588** is a targeted workaround for a GCC `format-truncation` diagnostic; useful implementation history but not a general Wireshark convention beyond existing compiler-warning guidance.
- **!22585** adds GSM SIM STORE DATA command reassembly using the existing APDU-response model; accepted feature work without a new cross-cutting convention.
- **!22584** renames the asn2wrs no-constraint-check option after constraint checking became the default; primarily CLI/tool consistency.
- **!22582** aligns UAT escape/unescape types with byte/string semantics; useful type cleanup already covered by type-domain guidance.
- **!22581/!22580** are release-preparation changes.
- **!22578** adds NMEA2000 PGN value-string data.
- **!22576** exports BER constrained helpers after generated dissectors began relying on constraint checking by default; useful API exposure but adequately covered by existing public API/header rules.
- **!22573** adds SMPP request/response tracking; substantive feature work but no sufficiently general new rule beyond existing conversation/reassembly state conventions.
- **!22569/!22568** are reverts and therefore evidence about the state of the tree rather than positive design guidance. **!22570** is a closed duplicate/alternate revert and was down-weighted.
- **!22565** is a draft alternative for repairing expert-field ABI breakage; down-weighted versus accepted ABI fixes reviewed elsewhere.
- **!22556** adds binary NMEA0183 dissection; feature-specific.
- **!22547** adds Procmon process-information parsing; feature-specific and not promoted absent a stronger cross-cutting maintainer lesson.
- **!22542** is an open draft adding libabigail ABI checking with a failed snapshot pipeline. Its review discussion is useful background, but it is deliberately not treated as accepted project architecture or tooling policy.

## Weighting notes

Merged master changes and explicit maintainer review were weighted most heavily. Release cherry-picks were used mainly as corroboration of already accepted master behavior. Closed, reverted, superseded, or still-draft/open MRs were reviewed but not promoted as authoritative conventions unless later accepted work independently established the same rule.
