# Review findings — Wireshark MRs 10412 through 10363

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs: every IID from 10363 through 10412 inclusive. Selection was checked against accumulated notebook tracking rather than inferred from range filenames. The historical 17571–17620 batch remains preserved and counted. Outcomes were 47 merged, one closed/superseded (10398), and two open (10385 and 10379); non-merged work was down-weighted.

## Durable findings

- MRs 10396, 10397 and 10407 establish the value-string fallback contract. A mismatched formatted fallback can fail at runtime; fixed-text fallbacks belong on the const helper. Martin Mathieson's checker turns the mechanically recognizable misuse into repository tooling while his backport discussion distinguishes reachable bugs from unreachable bad form.
- Guy Harris's master MR 10389, with 10391/10392 backports, explicitly separates TCP stream-framing entry points from complete-PDU media-type entry points. This strongly corroborates the notebook's framing architecture rule.
- Guy's MR 10371 moves the Internet media-type registry and context contract out of HTTP into neutral ownership, establishing that shared extension-point ownership should match the semantic namespace rather than its first carrier.
- Guy's master MR 10376, with 10380/10381 backports, uses separate COSE media-type adapters because the table's data argument has a different contract from the tagged-COSE path. MR 10382 complements this by propagating legitimate content context to HTTP heuristic children.
- Guy's 10393/10394 capture-open changes replace coarse error buckets with concrete semantic statuses; 10395/10408 add actionable Linux EAFNOTSUP guidance. The notebook now records classification before presentation as the durable rule.
- MR 10412 makes representative captures explicit project policy for non-trivial bugs and strongly encouraged evidence for enhancements. MR 10405 independently shows a merged new dissector being asked for a pcap and fuzzing evidence.

## Corroborating observations

John Thacker's MRs 10374 and 10363 add MPEG-PES stream-type dispatch and retain current PMT stream-type metadata by PID so payload interpretation follows declared stream type. MR 10403 fixes ownership leakage uncovered by static analysis in SSH key-material parsing. MR 10405 also demonstrates review of non-unique value mappings, standard field/checksum helpers, TVB access, release notes, and cross-platform portability.

Open MR 10385 contains useful but provisional discussion favoring a generic persistable UI layout model over more fixed layouts. Open MR 10379 exposed a Windows build failure in pipe-spawn logging. Closed MR 10398 was superseded by merged MR 10384. Those three were counted but not used as accepted architecture.

The authoritative exact set is recorded in `reviewed-mrs-automation/reviewed-mrs-automation-10363-10412.md`.
