# Run manifest: Wireshark MR review !1660-!1709

- Model requirement: satisfied with GPT-5.6 Sol.
- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook predecessor commit: `94a87f910a168dbe887e8a42724273fe58b40a6a`
- Reviewed count: **50**
- Exact range: **!1709 through !1660**, with every MR number in that range present in the corpus and selected.
- Status mix: **48 merged, 2 closed/unmerged** (!1698, !1663).
- Historical !17571-!17620 batch: revalidated from its dedicated ledger as **50 unique reviewed MRs** and preserved.
- Exact ledger: `reviewed-mrs-automation/ledger-1660-1709.md`
- Detailed findings: `reviewed-mrs-automation/review-findings-1660-1709.md`
- Notebook branch: `automation/mr-review-1660-1709-authoritative`
- Next frontier probe: `!1659`, “DoIP: Make finding start of message more robust”, merged on master; metadata only, not counted as reviewed.
- Corpus exhaustion: **no**. `mr_1659.json` exists at the reviewed corpus commit.
- SMPTE ST 291/VANC packet types encountered: **none**.

## Selection reconciliation

Selection was based on exact MR-number accounting rather than inferred contiguous coverage. The available tracking consulted included `reviewed-mrs.md`, the aggregate automation tracker, the per-run ledgers under `reviewed-mrs-automation/`, the completed low-number review branches, the predecessor exact !1710-!1759 ledger, and the dedicated historical !17571-!17620 ledger. The predecessor recorded !1709 only as an unreviewed frontier probe.

## Files changed by this run

- `text-encoding-conventions.md`
- `field-display-policy-conventions.md`
- `heuristic-dissector-conventions.md`
- `conversation-api-conventions.md`
- `subprocess-ipc-lifecycle-conventions.md`
- `dissector-resilience-conventions.md`
- `submission-backport-scope-conventions.md`
- `submission-conventions.md`
- `tap-listener-lifecycle-conventions.md`
- `reviewed-mrs-automation/ledger-1660-1709.md`
- `reviewed-mrs-automation/review-findings-1660-1709.md`
- `reviewed-mrs-automation/run-manifest-1660-1709.md`
