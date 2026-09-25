# Wireshark MR review findings: !9813–!9862

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed merge requests, working downward from !9862 through !9813. Candidate membership was checked against the latest accumulated notebook state, the per-run ledger inventory under `reviewed-mrs-automation/`, the aggregate automation tracking, and `reviewed-mrs.md`; no member of this batch had a prior substantive review entry. The historical !17571–!17620 ledger was also re-opened and verified to contain exactly 50 unique MR numbers.

Outcome weighting: 48 of the 50 MRs were merged. !9859 was closed after GitLab failed to rebase the release-3.6 backport; Guy Harris explicitly opened a fresh MR, which became merged !9860, so !9859 carries essentially no independent architectural weight. !9852 was a closed precursor to the same RTPS query-dissection work accepted in merged !9854, so the merged result is weighted more heavily.

## Strong durable findings

### Derived protocol-tree values must not pretend to occupy unrelated packet bytes — !9846, !9861, !9862

Guy Harris's merged master !9846 fixes Gryphon IOCTL response display. The IOCTL code is remembered from the request; it is not encoded in the response. The accepted implementation therefore gives the displayed value a zero-length TVBuff range rather than highlighting an arbitrary response byte, and fixes the field type from `FT_UINT8` to `FT_UINT32`. Guy's release-4.0 and release-3.6 backports !9861 and !9862 provide unusually strong corroboration.

Promoted to `protocol-item-source-range-conventions.md`.

### Do not allocate an advertised element count before validating the elements — !9830

John Thacker's merged RTPS fix addresses a fuzz-generated 32-bit element count that could drive a huge file-scope allocation before ordinary bounds handling stopped the parse. The accepted path starts a growable wmem array with a bounded initial capacity, appends only successfully parsed elements, then finalizes and trims it. This preserves support for large legitimate types without trusting a hostile count for speculative memory commitment.

Promoted to `count-driven-allocation-conventions.md`.

### Source, proxy, and displayed column numbers are different identities — !9819 and !9840

John Thacker's merged !9819 fixes timeline delegates that followed visual column slots after hidden/filter changes instead of Start/Duration semantics. Roland Knall's review sharpened the Qt API rule that a column operation should remain column-oriented rather than requiring an arbitrary row index. John's merged !9840 applies the complementary rule to filtering: map the selected proxy column to a stable source-model column when the filter is chosen, so later hiding does not change its meaning and the mapping still works when no rows are visible.

Promoted to `qt-proxy-column-identity-conventions.md`.

### Value-string tables and field widths are one semantic contract — !9837, !9858, !9860

Martin Mathieson's merged master !9837 fixes cases where `value_string` entries could not fit the registered field domain. The corrections include widening legitimately larger semantic fields, correcting erroneous numeric mappings and ranges, and retaining an explicitly documented GMR-1 exception rather than mechanically changing an ambiguous protocol case. Guy Harris's merged stable backports !9858 and !9860 reinforce that these were correctness issues; Guy's !9845 documents the GMR-1 ambiguity in detail.

Promoted to `value-string-domain-conventions.md`.

### Compatibility means both the declared C++ standard and the oldest supported Qt API — !9822, !9836, !9844

Merged !9822 introduced `std::as_const`, a C++17 API, into a project still requiring C++11. John Thacker's merged !9836 removed that dependency but still broke Qt 5 by assuming Qt 6-style `QList` size/index typing. Nicolás Alvarez's merged !9844 supplied the accepted C++11/Qt5-compatible range-for using `qAsConst`. The discussion is also useful CI evidence: supported-build failures escaped the normal pipeline, so baseline configurations need explicit coverage.

Promoted to `build-baseline-compatibility-conventions.md`.

## Additional corroborating evidence

- !9856 performs a broad accepted conversion from ambient `wmem_packet_scope()` to explicit `pinfo->pool`; helper APIs either receive an allocator when they are memory-oriented or a `packet_info *` when they are packet-oriented. This strongly corroborates the notebook's existing allocator-scope and explicit-context guidance.
- !9839, a new TRDP dissector, received direct review requesting both release-note coverage and a representative capture. The author supplied both. Martin Mathieson also noted that TCP support would require registration on `tcp.port` plus `tcp_dissect_pdus()`, while explicitly accepting UDP-only scope for the initial merge.
- !9824 evolves sshdump's `--remote-sudo` into a more general privilege-selection interface while retaining the old CLI for script compatibility and translating the old stored preference into the new value domain. This corroborates the existing preference and CLI migration rules.
- !9823 rejects TvbRange lengths below the documented `-1` sentinel; this independently reinforces validating sentinel domains before arithmetic or range construction.
- !9817's MySQL discussion contains useful John Thacker guidance that an ambiguous leading response byte cannot be classified reliably without the protocol state machine; state should outrank a local byte-prefix heuristic.
- !9818 preserves TCP's `REASSEMBLE_ENTIRE_SEGMENT` state when a newly arrived overlapping segment still fails to complete reassembly. State flags representing an outstanding reassembly obligation should clear only when the corresponding completion condition is actually reached.
- !9850 replaces an overly broad TLS GREASE test with an exact specification predicate, preventing values such as 0x1a2a from being discarded accidentally. This corroborates the rule that protocol classifiers should encode the full normative predicate rather than a convenient partial bit pattern.
- !9842 adds a specific RSVP structural field so JSON and filter output identify S2L_SUB_LSP correctly instead of exporting it under a generic unknown-object identity. Registered tree structure is part of machine-readable output, not only GUI presentation.
- !9825 fixes Qt model/proxy leaks by assigning QObject parents to objects owned by a single view, corroborating existing Qt ownership guidance.
- !9845 is strong source-comment evidence from Guy Harris, but the same master change had already been encountered through its stable backports !9863/!9864 in the immediately preceding batch, so no duplicate durable rule was added here.

## Down-weighted or superseded work

!9859 is a failed GitLab-rebase attempt superseded by merged !9860. !9852 is a closed precursor to the merged RTPS query-dissection work in !9854. Their diffs and discussions were reviewed for historical context, but durable conclusions are based on the merged successors.

## Next frontier

!9812 (`TCP: Don't clear REASSEMBLE_ENTIRE_SEGMENT if we didn't complete`) exists in the corpus, targets master, and is merged. Its metadata was inspected only to establish the next descending frontier; it was not reviewed or counted in this run.
