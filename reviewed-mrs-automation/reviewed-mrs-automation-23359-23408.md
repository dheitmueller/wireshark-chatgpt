# Automated Wireshark MR review: !23359-!23408

## Corpus identity

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
- Review direction: newest available previously-unreviewed MRs toward older MRs
- MRs reviewed in this run: **50**

## Selection and de-duplication

Before selecting this batch, the review-tracking material in `dheitmueller/wireshark-chatgpt` was consulted, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run files in `reviewed-mrs-automation/`. Candidate MR numbers were checked against the tracking individually rather than treating ledger filename ranges as proof that an entire numeric interval had already been reviewed.

The historical reviewed batch **!17571-!17620** remains part of the already-reviewed set and is preserved/countable through its existing automation ledger.

Repository-wide tracking checks found no previous review entries for the selected candidates. The fifty highest-numbered corpus MRs not already represented in the review tracking were therefore !23408 through !23359 inclusive.

## Exact MRs reviewed

1. !23408
2. !23407
3. !23406
4. !23405
5. !23404
6. !23403
7. !23402
8. !23401
9. !23400
10. !23399
11. !23398
12. !23397
13. !23396
14. !23395
15. !23394
16. !23393
17. !23392
18. !23391
19. !23390
20. !23389
21. !23388
22. !23387
23. !23386
24. !23385
25. !23384
26. !23383
27. !23382
28. !23381
29. !23380
30. !23379
31. !23378
32. !23377
33. !23376
34. !23375
35. !23374
36. !23373
37. !23372
38. !23371
39. !23370
40. !23369
41. !23368
42. !23367
43. !23366
44. !23365
45. !23364
46. !23363
47. !23362
48. !23361
49. !23360
50. !23359

## Durable findings promoted to the notebook

### !23392 — Keep repository-local developer configuration relocatable

Merged master MR !23392 changes the developer-environment setup to configure Git with relative paths so a workspace can be moved without invalidating repository configuration. Jaap Keuter authored the change and John Thacker approved and merged it.

Promoted to `python-tooling-conventions.md`: repository-owned hooks/helpers/configuration should use paths relative to a stable repository/configuration anchor when the consumer supports them, rather than unnecessarily baking the checkout's current absolute path into persistent configuration.

### !23374 — Commit-message validators must honor Git's scissors boundary

Merged master MR !23374 fixes Wireshark's commit-message checker for `git commit -v`. Git places the verbose diff below the `# -- >8 --` scissors marker and strips that section before creating the commit; therefore a validator reading the editable message buffer must not lint that transient diff as commit-message text. Martin Nyhus authored the change and Jaap Keuter approved and merged it.

Promoted to `submission-conventions.md`: commit-message validation should mirror Git's semantic message extraction and ignore content below the scissors boundary that cannot appear in the resulting commit.

## Strong corroborating evidence not duplicated as new rules

- **!23371** (merged, authored by John Thacker) fixes MySQL compression state so random-access GUI redissection does not depend on sequential traversal order. It strongly corroborates the existing notebook rule that persistent dissector state must be reproducible under arbitrary redissection order.
- **!23377** (merged, authored by John Thacker) adds QUIC STREAM out-of-order and retransmission handling. It reinforces the existing stateful-analysis guidance on reordering, retransmission, and state-machine behavior rather than introducing a materially distinct general rule.
- **!23383/!23384/!23378** restore a USB-HID sanity range check to prevent resource exhaustion. These reinforce existing hostile-input/resource-bounding guidance.
- **!23406** exposes a public-header C++ compatibility problem involving a C++ keyword, corroborating the already-recorded rule from the immediately preceding batch that public C headers advertised as C++ compatible must avoid C++-reserved identifiers.
- **!23400, !23395, !23394, !23387, !23386, !23380, !23376, !23368, and !23364** continue the accepted TVB unsigned-offset, explicit-length/remaining-length, and fetch-once migrations already represented in the notebook.

## Evidence weighting

Merged MRs were weighted more heavily than abandoned or incomplete work. In particular, **!23397** was closed without merge and **!23360** was still a draft in the corpus snapshot, so neither was treated as durable accepted project policy. Backports and closely related duplicates were used primarily as corroboration rather than as independent rules.

Maintainer authorship, approval, and merge decisions were weighted according to the notebook's reviewer-authority guidance; the strongest findings in this batch include direct John Thacker authorship/merge evidence and accepted maintainer workflow/tooling changes.
