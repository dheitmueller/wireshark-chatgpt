# Automated Wireshark MR review: !23309-!23358

## Corpus identity

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Review direction: newest available previously-unreviewed MRs toward older MRs
- MRs reviewed in this run: **50**

## Selection and de-duplication

Before selecting this batch, the available review tracking in `dheitmueller/wireshark-chatgpt` was consulted, including `reviewed-mrs.md`, the supplemental aggregate automation ledger, and the per-run files under `reviewed-mrs-automation/`. Candidate MR numbers were checked against the tracking individually rather than treating ledger filename ranges as proof that every MR inside an interval had been reviewed.

The historical reviewed batch **!17571-!17620** remains part of the already-reviewed set and was explicitly preserved and counted through `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md`.

The fifty highest-numbered corpus MRs not already represented in the review tracking were confirmed individually to be !23358 through !23309 inclusive. Low-information maintenance/spec-update MRs were scanned for purpose, outcome, discussion presence, and change shape; MRs with substantive architectural, correctness, build, or review evidence received deeper discussion/final-diff inspection.

## Exact MRs reviewed

1. !23358
2. !23357
3. !23356
4. !23355
5. !23354
6. !23353
7. !23352
8. !23351
9. !23350
10. !23349
11. !23348
12. !23347
13. !23346
14. !23345
15. !23344
16. !23343
17. !23342
18. !23341
19. !23340
20. !23339
21. !23338
22. !23337
23. !23336
24. !23335
25. !23334
26. !23333
27. !23332
28. !23331
29. !23330
30. !23329
31. !23328
32. !23327
33. !23326
34. !23325
35. !23324
36. !23323
37. !23322
38. !23321
39. !23320
40. !23319
41. !23318
42. !23317
43. !23316
44. !23315
45. !23314
46. !23313
47. !23312
48. !23311
49. !23310
50. !23309

## Durable findings promoted to the notebook

### !23332 — Distinguish dissector code from dissector data in source-file names

Merged master MR !23332, authored and merged by Michael Mann and reviewed by Jaap Keuter, renames data-only support translation units from `packet-*` to `data-*`. The MR explicitly states that files containing only value tables/data structures and no dissection should not look like packet dissector implementations; the final change spans 123 files, making this a repository-wide naming convention rather than a local preference.

Promoted to `dissector-source-file-conventions.md`: reserve `packet-*` for files that actually implement/register dissection, and use `data-*` for data-only dissector support modules.

### !23337 — Do not decode text and search it to reconstruct original packet offsets

Merged master MR !23337, authored by John Thacker and merged by Anders Broman, replaces broken SRVLOC text handling. The MR states that its UCS-2 conversion never worked and, importantly, that retrieving decoded text and then searching it to recover offsets fails with fuzzed input. The final diff explains that arbitrary/variable-width encodings make decoded-string coordinates unreliable as original-frame byte coordinates.

Promoted to `text-decoding-offset-conventions.md`: parse offsets in the buffer whose bytes are actually being interpreted. For variable-width/escaped/normalized text, parse directly with encoding-aware tvb APIs or create a derived tvb/data source and keep offsets in that derived coordinate space; do not rediscover source offsets by substring search after decoding.

### !23333 — Scope dependency-driven language standards to the targets that need them

Merged master MR !23333, authored and merged by Gerald Combs, changes CMake so C++17 is set on the individual targets that require libsinsp rather than globally. This keeps a stronger dependency-specific language requirement from needlessly raising the language mode of unrelated targets.

Promoted to `build-language-standard-conventions.md`: distinguish Wireshark's project-wide language baseline from stronger component/dependency requirements and apply the latter at the narrowest target scope that actually needs them unless the project deliberately raises the global baseline.

## Strong corroborating or contextual evidence not duplicated as new rules

- **!23345** (merged master, authored by John Thacker) fixes MySQL compression state under random-access GUI redissection and explicitly tests non-sequential frame-selection orders. It strongly reinforces the existing notebook rule that persistent dissector state must be valid under arbitrary redissection order rather than assuming a single forward pass.
- **!23347 and !23318** are further accepted double-fetch cleanups and reinforce the existing `proto_tree_add_item_ret_*` / fetch-once guidance rather than introducing a new rule.
- **!23331, !23325, and !23319** continue the accepted unsigned-offset / `expert_remaining` migration, while **!23323** guards a zero protocol length specifically because that value controls parser progress. These fit existing offset, remaining-length, and progress invariants.
- **!23353** and related backports **!23356/!23357** cache stable Qt child-row state to replace a repeated O(N) lookup with O(1). **!23355/!23358** optimize Expert Dialog proxy-model child checks and substantially reduce repeated sorting/mapping work. These are useful performance examples, but a substantive post-merge review on !23355 by Roland Knall warned that the patch masks part of the symptom while leaving model/proxy separation and future grouping flexibility unresolved; Anders Broman acknowledged that a better follow-up could replace it. The optimization was therefore not promoted as a general architecture rule.
- **!23321** reuses the existing RFC 7468 dissector for a PEM-encoded MySQL public key and reinforces the existing preference for shared decoders/helpers rather than local reimplementation.
- The numerous protocol/spec/version upgrades and focused packaging/UI maintenance changes in this batch were counted and reviewed but did not justify new cross-cutting notebook rules.

## Evidence weighting

Merged master changes were weighted above backports, closed attempts, drafts, and superseded submissions. In particular, **!23339** is a closed predecessor to merged !23340; **!23324** is a closed predecessor in the expert-offset cleanup series; **!23350** is the draft predecessor around the merged MCData work; and **!23320/!23327** are draft work around Packet Diagram behavior. They were retained as context but not treated as accepted project policy.

The strongest authority in this batch comes from merged work authored or accepted by John Thacker, Michael Mann, Gerald Combs, Anders Broman, and other established maintainers. No direct Guy Harris review in this selected batch was used to manufacture a higher-confidence rule; authoritative evidence was weighted according to who actually participated in each MR.
