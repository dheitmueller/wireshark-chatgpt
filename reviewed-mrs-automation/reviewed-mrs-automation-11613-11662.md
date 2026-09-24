# Wireshark MR review automation ledger: !11613–!11662

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection was made only after reconciling the available review tracking in this notebook, including `reviewed-mrs.md` and the per-run files under `reviewed-mrs-automation/`. The historical !17571–!17620 batch remains part of the already-reviewed set. Numeric ranges were not assumed to be complete merely because some entries in them appeared in a tracker. !11662 had previously been inspected only as a frontier probe and was therefore still eligible for this run.

## Exact reviewed MR set

The following fifty MRs were reviewed in descending order:

- !11662
- !11661
- !11660
- !11659
- !11658
- !11657
- !11656
- !11655
- !11654
- !11653
- !11652
- !11651
- !11650
- !11649
- !11648
- !11647
- !11646
- !11645
- !11644
- !11643
- !11642
- !11641
- !11640
- !11639
- !11638
- !11637
- !11636
- !11635
- !11634
- !11633
- !11632
- !11631
- !11630
- !11629
- !11628
- !11627
- !11626
- !11625
- !11624
- !11623
- !11622
- !11621
- !11620
- !11619
- !11618
- !11617
- !11616
- !11615
- !11614
- !11613

Status weighting: 49 were merged. !11615 was closed/unmerged and was down-weighted; it was a Guy Harris Qt QByteArray compatibility proposal that became unnecessary after the project's minimum-Qt policy moved to Qt 6.

## Durable findings promoted to the notebook

- **!11630 — lifecycle hook semantics (extremely high confidence).** Guy Harris authored the merged master documentation defining the intended repetition boundaries for init, cleanup, and shutdown callbacks. Init/cleanup participate in capture/dissection and preference-driven reinitialization; shutdown is one-time process/epan teardown. Added to `initialization-lifecycle-conventions.md`.
- **!11629 — conversation identity and entry-path initialization (extremely high confidence).** John Thacker's merged master SNMP fix shows that conversation keys must encode the protocol's actual correlation invariants rather than every concrete transport-tuple value. Replies may legitimately arrive from a different ephemeral port, so an over-specific key split request/response state and created extra Follow Stream conversations. The same MR fixes an SMUX path that entered shared SNMP handling without required conversation state. Added to `conversation-api-conventions.md`.
- **!11639 — malformed child lengths, containment, and parser progress (extremely high confidence).** John Thacker's merged master Bluetooth SDP fix demonstrates that replacing an oversized declared length with zero can turn malformed input into a no-progress loop. The accepted behavior reports the malformed length while bounding child parsing with a subset TVBuff so nested decoding cannot escape the parent element. Stable backports !11644 and !11645 reinforce the rule. Added to `tvbuff-parsing-conventions.md`.

## Strong corroborating evidence retained without duplicating existing rules

- **!11649**, with stable backport !11654, fixes a conversation hash table that survived capture-file changes while retaining keys allocated in file scope. This directly corroborates the existing resource-lifetime rule that retained container keys/referents must live at least as long as their owning container; the accepted fix moves the key to epan scope.
- **!11624** reinforces existing configuration-precedence guidance: `--only-protocols` and related command-line selections are invocation/session overrides of the effective profile configuration, not mutations of persistent/default-disabled protocol semantics. Profile changes in the GUI clear the transient command-line override layer rather than rewriting profile meaning.
- **!11657**, with backport !11659, is a useful offset-ownership example: `dissect_e212_mcc_mnc()` already returns the advanced offset, so adding the consumed length again double-advances the parser. Treat helper return contracts as authoritative rather than independently repeating their offset accounting.
- **!11648** corroborates the typed-item/static-checker convention that checker exceptions should model legitimate API structure precisely rather than weakening a useful check globally.
- **!11635** provides good submission/testing evidence for a new protocol extension: representative capture material, cross-platform testing, and 10,000 fuzz iterations were supplied; the author also deliberately limited scope where broader state handling would have required substantially more conversation machinery.
- **!11623** contains useful but more conditional Guy Harris review feedback around malformed/ambiguous protocol behavior: prefer automatic diagnosis/expert information when a broken encoding can be recognized reliably, but a preference can be appropriate when packet-local evidence cannot disambiguate the deployed variants safely.
- **!11655** flushes a pcap stream after writing the header so the consuming peer can make progress immediately. Useful streaming evidence, but too narrow to justify a separate notebook convention in this run.

## Down-weighted/superseded material

- **!11615** was closed without merge after the minimum Qt version moved to Qt 6, eliminating the compatibility problem the proposal addressed. It was reviewed for historical context but not used as durable accepted precedent.
- Routine generated-data updates, release-branch backports whose master changes carried the substantive lesson, narrow dissector additions, documentation-only cleanups, and warning/style fixes were reviewed but not promoted where they did not add a durable convention beyond material already in the notebook.

## Frontier

!11612 (`[Automatic update for 2023-08-06]`) exists in the same corpus commit and is merged. It was inspected only to verify that the corpus continues below this batch and is **not** part of the fifty reviewed MRs in this ledger. It remains eligible for the next descending run unless newer unreviewed corpus material appears.
