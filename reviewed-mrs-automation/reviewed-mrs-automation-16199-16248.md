# Wireshark MR review automation ledger: !16199–!16248

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook starting commit: `95e1e065723c1d76dc1f4df4e37bd063422b7aaf`

## Selection method

The batch was selected by enumerating the complete corpus Git tree, rebuilding the already-reviewed set from all available files under `reviewed-mrs-automation/` together with `reviewed-mrs.md` where applicable, explicitly preserving/counting the historical !17571–!17620 batch, subtracting that exact reviewed set from the corpus MR set, sorting the remainder numerically descending, and taking the highest 50 MR numbers. No contiguous numeric range was assumed reviewed merely from a neighboring ledger.

## Exact reviewed MR set

!16248, !16247, !16246, !16245, !16244, !16243, !16242, !16241, !16240, !16239,
!16238, !16237, !16236, !16235, !16234, !16233, !16232, !16231, !16230, !16229,
!16228, !16227, !16226, !16225, !16224, !16223, !16222, !16221, !16220, !16219,
!16218, !16217, !16216, !16215, !16214, !16213, !16212, !16211, !16210, !16209,
!16208, !16207, !16206, !16205, !16204, !16203, !16202, !16201, !16200, !16199

Count: **50**.

## Durable findings promoted to the notebook

- **!16218 + !16234 (merged; Guy Harris): API error-reporting ownership.** The file-merging path was first moved onto the standard reporting facade and then refactored so the merge API itself reports operation-intrinsic failures instead of requiring every caller to reconstruct the same diagnostic. Caller-specific control flow remains at the caller. Recorded in `api-error-reporting-conventions.md`.
- **!16225 + !16205 (merged): auto-reset wmem container lifetime.** When the container root is epan-scope but its contents reset at file scope, create the root once at protocol registration; recreating it during per-file initialization leaks persistent container metadata. Recorded in `wmem-container-lifetime-conventions.md`.
- **!16228 (closed/superseded) + !16245 (merged): generated dissector workflow.** The rejected predecessor edited generated ITS output directly. Maintainer review redirected the change to the ASN.1 source of truth, and the corrected merged MR included both the authoritative input change and regenerated checked-in `packet-its.c`. Recorded in `generated-code-conventions.md`. The closed predecessor is used only as negative/superseded evidence; the merged replacement carries the implementation weight.
- **!16237 (merged): hostile count validation before allocation.** NFSv4 malformed input could request roughly 2.2 GiB through a count-driven wmem array allocation. The accepted fix checks the count before allocation across all equivalent decode paths, with a reproducer and Valgrind evidence. Recorded in `input-resource-limit-conventions.md`.

## Other useful evidence reviewed

- **!16241 (merged; Guy Harris):** initialize diagnostic/error bookkeeping defensively when compilers cannot prove that invariants establish values on every path, particularly where assertions may disappear in release builds.
- **!16226 (merged):** after a specialized writer path successfully emits a Simple Packet Block, control flow must not fall through to the generic Enhanced Packet Block writer and duplicate the logical packet.
- **!16223 (merged):** the C99 `bool` conversion cannot be applied blindly at GLib callback boundaries that require `gboolean`, corroborating the notebook's existing dependency-API type-boundary rule.
- **!16221 (merged):** expected permission-related interface-scan failures for non-root users are debug-level diagnostics rather than warning spam.
- **!16219 (merged; Guy Harris):** short-read handling depends on the exact Wiretap error sentinel; a reversed equality test changes parser behavior.
- **!16217 (merged):** internal record-number width should reflect the subsystem's actual capacity rather than inherit a narrower UI/application limit when Wiretap itself does not require that limit.
- **!16214 (merged):** lexer bookkeeping must advance source line state while consuming comments so parser diagnostics identify the real source location; EOF syntax-error reporting must also handle a missing token value safely.
- **!16211 (merged):** Valgrind-identified per-packet fields that can be read outside their conditional assignment path should be initialized when the containing state is allocated.
- **!16199 (merged):** maintainer review caught inconsistent signed/unsigned I/O-graph throughput calculations; the accepted version made the parallel calculation paths consistent and documented the new graph unit.

Closed/superseded work such as !16230 and !16228 was deliberately weighted below merged master changes and used only where it explains an accepted correction or submission rule.

## Notebook commits made during this run

- `94d627c22bfd35cdf5e6dcad3aa70639a3165ae4` — record auto-reset wmem container registration lifetime.
- `0605973a6018beff201094f93c98dfbfce2d6b27` — record generated dissector submission workflow.
- `c880f61ed68ec551e9eee247fed146a3683f2921` — record merge API error-reporting ownership.
- `d42f445f2b9ad6e44d68d6132838e2f36e5ddfce` — record count validation before allocation.

This ledger records exactly the 50 MRs reviewed in this run and the corpus snapshot used.