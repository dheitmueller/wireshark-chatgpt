# Automated Wireshark MR review: !18259-!18308

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: newest to older. Before selecting this batch, existing per-run ledgers in `reviewed-mrs-automation/` and `reviewed-mrs.md` were consulted. The historical !17571-!17620 batch remains part of the already-reviewed set. The immediately newer automation ledger covers !18309-!18358, so the fifty highest-numbered corpus MRs not already recorded as reviewed are exactly !18308 down through !18259.

## Exact reviewed MR set

!18308, !18307, !18306, !18305, !18304, !18303, !18302, !18301, !18300, !18299, !18298, !18297, !18296, !18295, !18294, !18293, !18292, !18291, !18290, !18289, !18288, !18287, !18286, !18285, !18284, !18283, !18282, !18281, !18280, !18279, !18278, !18277, !18276, !18275, !18274, !18273, !18272, !18271, !18270, !18269, !18268, !18267, !18266, !18265, !18264, !18263, !18262, !18261, !18260, !18259.

Count: **50**.

## Weighting and durable findings

Merged MRs were treated as stronger evidence than closed/superseded work, and maintainer-authored/accepted fixes were weighted accordingly.

- !18308 and !18307 are release-branch backports of the NR/LTE RRC `AbsoluteTimeInfo` correction. They preserve the generated-code discipline seen in the master fix: the ASN.1 `.cnf` source and generated dissector are changed together, and the BCD conversion uses the correct big-endian helper. This corroborates existing generated-code/source-of-truth guidance rather than adding a new rule.
- !18300 is a merged John Thacker display-filter hardening fix. `oper_dup()` had assumed every operation was binary; a chained comparison containing unary minus could therefore duplicate a NULL second operand and crash under ASan. The fix treats the AST node's actual arity as authoritative. This is useful defensive-library evidence: generic tree transforms must preserve and respect the grammar/node shape rather than assuming all operators have the same number of children.
- !18280 is a closed duplicate of !18279 and is weighted only as supersession context. Both fix a PTP output-argument bug where code tested `*tree_out`/`*ns_out` instead of testing whether the output pointers themselves were non-NULL. !18279 is the merged authoritative version.
- !18270 is the merged clean successor to !18260. The actual code change adds `packet-ipv6.h` to `DISSECTOR_PUBLIC_HEADERS` after installed public headers began including it. This corroborates the principle that exported/public headers must have a self-consistent install/export closure. The !18260 discussion is useful submission-workflow context: the original contributor branch/configuration prevented maintainers from pushing and complicated the pipeline; the work was resubmitted cleanly as !18270 and merged.
- !18259, by John Thacker, fixes UDP multicast bandwidth accounting to use UDP datagram length rather than the frame length associated with the tap. With fragmented IP, only the reassembled packet's final fragment frame was otherwise counted. The MR explicitly discusses why lower-layer overhead is not a stable semantic quantity across fragmentation, tunneling, encapsulation, and different capture link types. This is a strong measurement-design exemplar: choose the layer whose quantity matches the statistic's semantics, rather than using an easily available enclosing-frame size that becomes wrong under reassembly. It also widens cumulative byte accounting to 64 bits.

## Notebook impact

No additional convention file was changed in this run. The strongest findings either corroborate conventions already represented in the notebook (generated-code source of truth, semantic length/accounting, public API/export consistency, defensive handling of optional pointers/tree shapes) or are sufficiently narrow that a new durable rule would add noise rather than guidance.

## Continuation

On the next run, rebuild the already-reviewed set from all ledgers rather than assuming ranges. If no higher-numbered holes are discovered, continue backward below !18259. Preserve/count !17571-!17620.
