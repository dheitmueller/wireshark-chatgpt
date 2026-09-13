# Wireshark MR automation review: !21039-!21088

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Notebook repository: `dheitmueller/wireshark-chatgpt`

## Selection and duplicate avoidance

Before selecting this batch, the existing per-run files under `reviewed-mrs-automation/` and the sparse `reviewed-mrs.md` ledger were consulted to reconstruct the already-reviewed MR set. Numeric-range coverage was not inferred from partial ledger entries. The historical !17571-!17620 batch remains part of the reviewed set and is preserved.

The previous automation ledger ends at !21089. All 50 corpus records !21088 through !21039 exist at the pinned corpus commit and were absent from the reconstructed reviewed set, so they are the fifty highest-numbered previously unreviewed MRs available in the corpus.

## Exact reviewed MR set

`!21088 !21087 !21086 !21085 !21084 !21083 !21082 !21081 !21080 !21079 !21078 !21077 !21076 !21075 !21074 !21073 !21072 !21071 !21070 !21069 !21068 !21067 !21066 !21065 !21064 !21063 !21062 !21061 !21060 !21059 !21058 !21057 !21056 !21055 !21054 !21053 !21052 !21051 !21050 !21049 !21048 !21047 !21046 !21045 !21044 !21043 !21042 !21041 !21040 !21039`

Count: **50**.

## Weighting and durable findings

Merged master changes and direct maintainer review were weighted most heavily. Stable-branch cherry-picks/backports were treated mainly as corroboration. Closed, draft, or still-open work was down-weighted and was not promoted as accepted architectural precedent.

- **!21083** (merged master, John Thacker) and **!21086** (release-4.6): a partial Bluetooth HCI vendor-table split was reverted because the shipped vendor dissectors had not been migrated to the new payload/context contract. Durable rule promoted: treat a dissector-table contract change as a whole-consumer migration; do not leave in-tree consumers on an incompatible old contract.
- **!21065** (merged master, John Thacker; merged by Michael Mann): removed a vendor-specific Android preference because the existing Decode As table already provided the same selection generically. Durable rule promoted: prefer generic Decode As dispatch over a one-protocol preference that duplicates it.
- **!21063** (merged master; detailed John Thacker review): corrected HTTP header-map lifetime tracking. John explicitly identified both a packet-scope object escaping into file-scope conversation data and the opposite problem of retaining all HTTP headers for the full file lifetime. Durable rule promoted: persistent state and everything it references must have compatible lifetime, while still choosing the narrowest scope that avoids unnecessary long-session growth.
- **!21054** (merged master documentation): clarified Wiretap `wth->priv` ownership. Wiretap directly frees the private object, but separately allocated objects referenced by that structure require format-specific cleanup through `subtype_close`. Durable ownership rule promoted.
- **!21066** (merged master, approved/merged by John Thacker): fixed HTTP/2 state mutation ordering; the previous code overwrote the old window size before deriving the delta, making the delta zero. Durable state-machine rule promoted: consume old state before overwriting it when dependent transitions require the old value.
- **!21071** (merged master): new DECT NR+ MAC dissector included a representative capture explicitly offered for testing/fuzzing. This strongly corroborates the existing notebook preference for sample captures with substantive dissector additions, but did not require a duplicate rule.
- **!21082** (merged master, merged by Anders Broman): review led to using standard `value_string` machinery rather than ad-hoc value presentation, corroborating existing shared-idiom/API guidance.
- **!21059** (merged master, Coverity): made the JSON-event iteration condition enforce non-NULL before dereference. Corroborates existing API-return/precondition checking guidance.
- **!21058** (merged master, Coverity): corrected use of `time_t` where the rtpdump wire format is explicitly 32-bit, corroborating semantic-domain/type-width portability guidance.
- **!21084/!21085**: master fix and stable backport registering missing GSM SIM fragment subtrees; correctness/registration fix, no new general rule beyond registration completeness.
- **!21088/!21087**: falcodump no-interface exit-code/test-harness fix and backport. Later discussion in the corpus snapshot indicates placement still needed adjustment, so these were not promoted into a broad rule from this snapshot.
- **!21078** remains open and requested a small reproducing capture for its state-tracking fault; useful review evidence, but its proposed state-list implementation was down-weighted because it was not merged.
- **!21070** was closed and was down-weighted.
- **!21056** and **!21039** were draft/provisional work in the corpus snapshot and were down-weighted rather than treated as accepted precedent.

The remaining MRs in this batch were inspected for their diffs, disposition, and available discussion. They primarily consisted of release/version preparation, generated/registry data updates, localized build/test portability fixes, documentation/comment cleanups, focused protocol corrections, and stable-branch backports. They either corroborated conventions already present in the notebook or did not establish a sufficiently general review rule to justify another durable notebook entry.
