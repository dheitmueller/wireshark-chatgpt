# Automated MR review ledger: !10050-!10099

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `08420a2f7851841b7a38714f642987c8cd167184`
Review direction: descending MR number (newest available previously-unreviewed MRs first)

## Exact reviewed set

!10099, !10098, !10097, !10096, !10095, !10094, !10093, !10092, !10091, !10090,
!10089, !10088, !10087, !10086, !10085, !10084, !10083, !10082, !10081, !10080,
!10079, !10078, !10077, !10076, !10075, !10074, !10073, !10072, !10071, !10070,
!10069, !10068, !10067, !10066, !10065, !10064, !10063, !10062, !10061, !10060,
!10059, !10058, !10057, !10056, !10055, !10054, !10053, !10052, !10051, !10050.

Count: 50.

## Review notes

The complete existing automation tracking set and `reviewed-mrs.md` were treated as the already-reviewed authority; in particular, the historical !17571-!17620 batch remains counted. Selection was by exact MR membership, not by assuming numeric gaps were reviewed.

High-value durable evidence from this batch:

- !10089 (master) and accepted release backports !10096 (4.0) and !10097 (3.6), authored by John Thacker, repair a global RPC-over-RDMA pointer that referenced packet-scoped storage after frame teardown. The stable-compatible fix registers a frame-end routine to NULL the alias; the MR explicitly identifies protocol data as the cleaner long-term architecture. This was added to `allocator-scope-conventions.md`.
- !10098 adds the missing IEEE 802.11 WNM Notification Response dissector and supplies a concrete encrypted sample capture plus the TK and packet number needed to reproduce validation. Alexis La Goutte approved/merged it. This corroborates the notebook's existing capture-backed dissector-validation convention. Martin Mathieson also questioned use of `value_string_ext` for a small value set, preferring ordinary `value_string`/`VALS()` when extension machinery is unnecessary.
- !10095 fixes an ORAN Ext11 zero-count edge case by guarding `bundles[num_bundles-1]` behind `num_bundles > 0`, reinforcing the existing arithmetic/container-boundary rule that zero-element cases must be valid before deriving a last-element index.
- !10090 fixes a Valgrind-reported HL7 uninitialized-read path by zero-initializing the full `struct msh`; Gerald Combs authored and merged the master fix. !10099 is the corresponding accepted 4.0 backport. This corroborates existing initialization/memory-safety guidance.
- !10060 fixes a Wi-SUN display regression by attaching PHY-mode fields to the intended subtree rather than the outer tree, a useful example of preserving protocol-tree hierarchy as part of UI correctness.
- !10050 adds rtpdump reading support in Wiretap and MIME handling. Guy Harris's review is particularly authoritative: the external rtptools project defines the rtpdump format, not Wireshark; if Wireshark emits a different format that rtpdump/rtpplay do not consume, that is a Wireshark bug rather than a separate dialect to normalize in documentation. The accepted MR description was corrected to describe generic rtpdump files and the implementation accounts for writer differences without redefining the external format. This reinforces the existing architectural principle that external-format ownership and interoperability contracts should be respected rather than described from a Wireshark-centric viewpoint.
- Automated number/database update MRs in the batch (for example !10080) were treated as low-value convention evidence even when merged; generated-data churn is not a useful coding-style exemplar.
- Release-branch cherry-picks were weighted as confirmation that a fix was considered stable-worthy, but the corresponding merged master change was preferred as architectural evidence when both were present.

No abandoned or superseded proposal was treated as stronger evidence than a merged implementation.

## Notebook changes

Updated `allocator-scope-conventions.md` with the durable rule that longer-lived aliases must not retain pointers into packet-scoped storage after frame teardown; prefer protocol/packet data, with frame-end alias invalidation as a stable-compatible fallback.

## Continuation

On the next run, rebuild the exact already-reviewed set from all tracking files before selecting anything. With this corpus snapshot and current tracking, the next descending candidate is !10049 if present and not otherwise recorded. Do not infer that any numeric gap outside explicit ledger membership has been reviewed.