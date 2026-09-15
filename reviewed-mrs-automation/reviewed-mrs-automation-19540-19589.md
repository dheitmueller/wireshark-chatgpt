# Automated MR review ledger: !19540-!19589

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest available unreviewed MR toward older MRs.

Exactly 50 MRs were reviewed in this run. Existing review tracking was consulted before selection, including `reviewed-mrs.md`, all available `reviewed-mrs-automation/` ledgers, and the historical !17571-!17620 batch. The corpus commit is unchanged from the preceding runs, so there were no newly populated higher-numbered holes to displace this batch.

## Exact reviewed set

!19589, !19588, !19587, !19586, !19585, !19584, !19583, !19582, !19581, !19580,
!19579, !19578, !19577, !19576, !19575, !19574, !19573, !19572, !19571, !19570,
!19569, !19568, !19567, !19566, !19565, !19564, !19563, !19562, !19561, !19560,
!19559, !19558, !19557, !19556, !19555, !19554, !19553, !19552, !19551, !19550,
!19549, !19548, !19547, !19546, !19545, !19544, !19543, !19542, !19541, !19540.

## Review notes

The batch is dominated by merged maintenance fixes and release backports. Each MR was scanned for purpose/outcome and reusable engineering signal; higher-signal changes received deeper diff/discussion inspection. Merged changes were weighted above abandoned/superseded work, and no low-signal backport was promoted into a convention merely because it appeared in the batch.

High-value evidence:

- **!19576 — QUIC: Use connection ID, not ports and addresses, for reassembly.** Merged release-4.4 backport. The custom reassembly key uses protocol-semantic identity (connection number, stream ID, direction, reassembly ID) rather than mutable `packet_info` endpoint addresses/ports. The rationale explicitly covers QUIC endpoint migration and encapsulated dissectors changing packet address/port state. This independently corroborates the state-identity rule already learned from !19625 and was added to `state-and-lifecycle-conventions.md`.
- **!19578 — Drop invalid malloc attribute from wmem_tree_new*.** Merged release-4.4 backport. GCC LTO exposed a null dereference because `G_GNUC_MALLOC` asserted stronger non-aliasing semantics than the returned wmem tree actually satisfied. This strongly corroborates the existing notebook rule that compiler attributes are semantic contracts and incorrect annotations can make optimizer-visible false promises.
- **!19540 / !19542 / !19543 — command-line preference buffer-overrun fix and stable backports.** ASan supplied a concrete heap-buffer-overflow reproducer involving `wireshark -otls.keylog_file:/dev/null`; the fix was validated across master and supported stable branches. This corroborates existing reproducer-driven sanitizer and bounded-comparison guidance.
- **!19569 — wiretap LZ4 linked-block context reset.** Merged fix reinforces that seek/restart operations on stateful compressed streams must restore decoder state appropriate to the new logical position rather than reusing stale state.
- **!19572 — pcapng initial-block EOF handling.** Merged fix reinforces respecting authoritative EOF state, particularly for live compressed files where attempting another block read can otherwise block waiting for data that will not arrive.
- **!19546 — Conversations: Don't test unreachable conversations.** Merged cleanup of historical `NO_PORTS`/`NO_PORT_X` confusion; useful evidence that conversation flag semantics affect both correctness and search cost, but no new general rule beyond existing conversation/state guidance was warranted.
- **!19547 — PROFINET payload without AR.** Merged dissector enhancement; useful protocol-specific fallback behavior, but not generalized into a notebook convention.
- **!19580 / !19581 / !19582 / !19584 / !19586 / !19588.** TLS, ICMP, DECT, and expert-tap correctness/encapsulation fixes were reviewed for reusable lessons; their durable points were already represented by existing bounds, field-modeling, helper-contract, and state-ownership guidance.

The remaining MRs were protocol registry/value updates, UI/documentation/build cleanup, static-analysis fixes, backports, or narrow correctness changes with no additional durable convention beyond material already present in the notebook.

## Notebook changes

`state-and-lifecycle-conventions.md` was updated to add !19576 as independent merged evidence for choosing stable protocol-semantic identity for reassembly/state rather than mutable transport endpoint state.

Notebook update commit: `360569dcedcb7c154fd4567f19c14cf8bc70ff1c`
