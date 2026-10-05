# Wireshark MR Corpus Selection Conventions

This file records durable rules for selecting automation review batches accurately. These rules concern corpus and notebook bookkeeping rather than Wireshark implementation style.

## Reconstruct reviewed membership from exact entries across all available review history

The automation history is not guaranteed to be linear in one branch. Review ledgers can exist on sibling automation/mr-review-* branches, so a branch-local checkout can omit valid prior tracking.

Rule: before selecting a batch, union exact reviewed MR numbers from all available review tracking that can materially affect selection: reviewed-mrs.md, aggregate tracking, per-run files under reviewed-mrs-automation/, and relevant review branches. Do not infer membership from a filename range, and do not assume the current branch contains every prior ledger.

This reconciliation exposed both duplicate-review risk from sibling-branch tracking and holes inside otherwise contiguous-looking ranges.

## Do not interpret an empty normal file read as an empty Git blob

Large corpus JSON files can exceed the size handled normally by a GitHub Contents-style read. In this corpus, normal file reads returned an empty string for MR JSON files even though the recursive Git tree reported large non-empty blobs and direct blob reads returned valid JSON.

Recovered examples:
- mr_15376.json: roughly 1.3 MiB;
- mr_701.json: roughly 1.4 MiB;
- mr_261.json: roughly 1.7 MiB.

These records had previously been treated as empty/unreviewable and skipped.

Selection rule: if a corpus path exists in the recursive tree but a normal file-content read returns empty or implausible content, inspect the tree entry and blob SHA/size before classifying it as empty. For large objects, read the blob directly. Only treat a record as genuinely empty or invalid after verifying the Git object itself.

Why it matters: batch correctness is exact set subtraction over actual corpus MR records. A retrieval-layer size limit must not silently remove an MR from that set.

Confidence: very high. Three independently skipped records were recovered from the exact pinned corpus commit with this procedure.
