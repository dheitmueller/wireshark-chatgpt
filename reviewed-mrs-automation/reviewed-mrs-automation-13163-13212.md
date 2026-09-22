# Wireshark MR automation review: !13163 through !13212

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This ledger records the exact set completed by this run. Selection was rebuilt from the available `reviewed-mrs.md` and `reviewed-mrs-automation/` tracking rather than inferred from numeric ranges. The historical !17571-!17620 batch remains part of the already-reviewed set. The immediately preceding attempt had read this same candidate batch but failed before writing any notebook commits or ledger; because no completion ledger existed, these MRs remained unreviewed for tracking purposes and were deliberately retried here.

Merged/accepted outcomes were weighted more heavily than abandoned or superseded work. Maintainer feedback was weighted by authority, while trivial mechanical changes were not promoted into broad conventions merely because they were authored by a senior maintainer.

## Exact reviewed MR set

!13212, !13211, !13210, !13209, !13208, !13207, !13206, !13205, !13204, !13203, !13202, !13201, !13200, !13199, !13198, !13197, !13196, !13195, !13194, !13193, !13192, !13191, !13190, !13189, !13188, !13187, !13186, !13185, !13184, !13183, !13182, !13181, !13180, !13179, !13178, !13177, !13176, !13175, !13174, !13173, !13172, !13171, !13170, !13169, !13168, !13167, !13166, !13165, !13164, !13163

Count: **50**.

## Durable findings promoted

- **!13202 — Deep, merged master, John Thacker.** Clang 17 exposed decade-old ISDN supplementary-service registration code whose loop registered `isdn_sup_global_op_tab[i]` while its predicates repeatedly inspected element zero. Promoted to `dissector-registration-conventions.md`: table-driven registration conditions, discriminators, and handlers must all come from the same indexed record; seemingly surprising unreachable-code diagnostics can expose a stale/mismatched loop predicate.
- **!13197 — Deep, merged master, John Thacker; !13201 — merged release-4.2 backport.** HTTP/3 per-packet header state could collide because a subset-local offset was not globally unique, and even a raw source offset could collide across multiple decrypted QUIC data sources in one frame/layer. Promoted to `keyed-state-container-conventions.md`: state identity must include all dimensions needed to distinguish coexisting instances, such as stable data-source identity plus source-relative offset.
- **!13198 — Deep, merged master, John Thacker.** QPACK fast-path parsing returned successfully without setting a requested Huffman flag, and several tree highlights mixed nested/local and tvbuff-relative coordinate systems. Promoted to `parser-api-status-outparam-conventions.md` and `tvbuff-coordinate-conventions.md`: every successful path must honor requested output parameters, and tree start/length ranges must use the coordinate system of the tvbuff being displayed.

## Strong corroboration intentionally not duplicated

- **!13212 — merged master, John Thacker.** Strongly corroborates the existing logging convention that the custom `WS_LOG_DOMAIN` definition/header must take effect immediately after `config.h`, before another include can transitively pull in `wsutil/wslog.h`. The notebook already records this rule, so no duplicate section was added.
- **!13209 / !13210 — merged master/backport, John Thacker.** SNMP DateAndTime formatting was applied consistently to the alternate branch and stable branch. Useful correctness/backport evidence, but no additional cross-cutting rule beyond existing consistency/generated-source practices.
- **!13191 / !13192 — merged Guy Harris-authored stable backports.** These are mechanical final-newline fixes. Guy Harris's authorship is highly authoritative, but the changes are too mechanical to justify a new architecture or review convention; they were therefore counted and weighted appropriately without overgeneralization.

The remaining MRs in the exact set were examined for implementation, discussion, merge outcome, and reusable review evidence. They did not justify additional durable notebook guidance beyond the promoted/corroborating items above.

## Frontier

!13162 was inspected only as a frontier check after this batch. It exists in the same corpus commit and is merged, but it is **not** part of this reviewed set and must remain eligible for the next descending batch.
