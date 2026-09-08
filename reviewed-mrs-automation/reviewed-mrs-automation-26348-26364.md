# Automated Wireshark MR review: !26348–!26364

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook base before this run: `f68a9a2b5291cb4f4be6c43ddb515e2e03f14e45`

Review scope: exactly 17 previously unreviewed merge requests, !26348 through !26364 inclusive. The persistent notebook ledger already records !26365–!26393 as reviewed, so those MRs were deliberately excluded even though the automation ledgers had previously stopped at !26347. The corpus snapshot contains no `mr_26394.json`, so no further previously unreviewed MRs were available in this corpus commit. Merged MRs and accepted stable-branch backports were weighted more heavily than open proposals. Maintainer authority was weighted by project/domain expertise, with especially strong weight for Gerald Combs, Anders Broman, Pascal Quantin, Stig Bjørlykke, and other established maintainers appearing in this window.

## Durable findings promoted

- !26354: synthetic extcap bookmark names must not be parsed by blindly splitting on a separator when the externally supplied interface name can contain that separator. Gerald Combs changed the implementation after review to resolve the full synthetic name through the extcap registry and follow explicit parent identity. Promoted to `extcap-discovery-conventions.md`.
- !26355: API/container shape should encode real cardinality. John Thacker removed an impossible singleton outer `GList`; Gerald Combs approved and merged it. Promoted to `container-api-conventions.md`.
- !26349/!26350: Windows/MSYS2 workflows can legitimately produce either path separator. Ask structural path questions through path helpers rather than string suffix parsing. Promoted to `filesystem-path-security-conventions.md`.
- !26362: archive-import security must preserve full-width ZIP64 sizes until bounds are checked and must normalize prospective extraction paths before verifying containment within the trusted root. The master fix was approved/merged by Anders Broman; the same design was subsequently accepted on stable branches. Promoted to `filesystem-path-security-conventions.md`.
- !26360: custom reassembly length accumulation must use checked arithmetic and respect downstream signed-size limits. This strengthens the reassembly rule already present in `dissector-conventions.md`; no duplicate section was added.
- !26353: warnings from extcap parsing belong in the logging system rather than ordinary stdout. This corroborates the notebook's existing rule that stdout/stderr have established process-stream semantics and should not be casually repurposed; no duplicate rule was added.

## Exact MR accounting

| MR | State at corpus snapshot | Weight | Review disposition |
|---|---|---|---|
| !26348 | merged | high | Release-4.6 backport of frame-comment exception-boundary fix; corroborates already-recorded rule that protocol-tree construction can raise dissector exceptions. |
| !26349 | merged | very high | John Thacker MSYS2 test-path normalization; promoted cross-platform path-representation lesson with !26350. |
| !26350 | merged | very high | John Thacker replaces separator-sensitive suffix parsing with basename helper; promoted. |
| !26351 | merged | high | Release-4.4 backport of frame-comment exception-boundary fix; corroborating evidence only. |
| !26352 | merged | high | Materializes a `Path.glob()` iterator before pytest parametrization so the test data can be reused; test-framework-specific cleanup, no new broad convention. |
| !26353 | merged | very high | Extcap parser sends warnings through logging instead of stdout; corroborates existing process-stream/output-contract guidance. |
| !26354 | merged | extremely high | Gerald Combs extcap bookmark identity change, revised after separator-collision review; promoted explicit-identity/registry-lookup rule. |
| !26355 | merged | extremely high | John Thacker removes impossible singleton outer configuration list; Gerald Combs approval/merge; promoted real-cardinality container rule. |
| !26356 | merged | high | O-RAN SE16 count aligned with UE-ID entries; protocol-specific correctness fix, no generic convention extracted. |
| !26357 | merged | very high | NAS EPS Info-column correction merged by Pascal Quantin; protocol-specific presentation fix. |
| !26358 | opened | provisional | Packet-list frozen columns/row pinning remains unmerged at this snapshot; not used as accepted UI architecture evidence. |
| !26359 | merged | very high | Extcap flag parsing corrected so Windows helpers such as etwdump can shut down properly; implementation-specific lifecycle fix, consistent with existing extcap rules. |
| !26360 | merged | very high | John Thacker AVCTP checked-add/cap for custom reassembly; strengthens existing overflow/standard-reassembly guidance. |
| !26361 | opened | provisional | TCP XMAS/NULL/SYN+FIN expert-info proposal includes tests but remains unmerged; no convention promoted. |
| !26362 | merged | extremely high | John Thacker profile-ZIP traversal and ZIP64-size hardening, approved/merged by Anders Broman; promoted archive path/size security rules. |
| !26363 | merged | high | Stratoshark release-note update; no engineering convention. |
| !26364 | merged | high | Release-4.6 backport of !26360 AVCTP reassembly overflow fix; corroborating stable-branch evidence. |

## Notebook files changed in this run

- `extcap-discovery-conventions.md`
- `container-api-conventions.md`
- new `filesystem-path-security-conventions.md`
- this ledger file

The next numeric MRs present in this corpus snapshot, !26365–!26393, were already recorded as reviewed in the persistent `reviewed-mrs.md` ledger and were therefore not re-reviewed. The corpus snapshot ends at !26393, so this run correctly stopped at 17 rather than filling the requested maximum of 50 with duplicates or non-corpus data.
