# Review findings: !7711-!7760

Corpus: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 MRs, !7760 through !7711. Forty-five are merged. The five closed/unmerged MRs are !7746, !7744, !7738, !7737, and !7722; they are retained only for review/process lessons and are weighted below merged work.

## Promoted findings

- **!7754-!7760 — Guy Harris:** preserve captured evidence rather than silently normalizing inconsistent reported/captured lengths. A known Linux USB producer bug is repaired in the format-specific reader; unexplained inconsistency remains visible as an expert diagnostic.
- **!7755 — John Thacker:** adding reset-token metadata to `quic_cid_t` exposed a connection lookup regression because hashing depended on structure layout. The accepted implementation hashes semantic CID bytes, matching equality.
- **!7749 — John Thacker:** packet-reachable TLS vector bounds failure becomes ordinary malformed-input handling, while a true programmer invariant remains an assertion.
- **!7747 / !7741 — Tomasz Moń:** child-process completion is asynchronous lifecycle state. Capture shutdown requests extcap completion first, observes callbacks, uses bounded fallback handling if needed, and only then completes dependent capture shutdown.
- **!7743 — John Thacker:** internal column state belongs in the internal column header; dissectors use the public utility surface instead.
- **!7739 — John Thacker:** the sample dissector is best-practice documentation: named dissector registration, automatic preferences, port ranges when appropriate, and no arbitrary default port assignment.
- **!7732 — Martin Mathieson:** `check_typed_item_calls.py` grows a check for consecutive duplicate protocol-tree additions and removes real duplicate/hidden-visible mistakes.
- **!7728 — Pascal Quantin review:** third-party crypto support is gated by the API available at build time; missing dependency constants are not locally invented.
- **!7727 — John Thacker:** every Expert Info model mutation that requires refresh requests a redraw; an earlier dirty state can be cleared during a long retap.
- **!7719 — John Thacker:** substantive dissection cannot depend on whether a protocol tree is present.
- **!7711 — Daniël van Eeden, with Kaige Ye review:** MySQL query attributes depend on effective bilateral capability; the client flag alone does not establish the extended wire layout.

The complete durable extraction is in `reviewed-mrs-automation/conventions-7711-7760.md`; the authoritative exact reviewed set is in `reviewed-mrs-automation-7711-7760.md`.
