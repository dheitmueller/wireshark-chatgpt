# Automated Wireshark MR review: !19790-!19839

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest available unreviewed MRs toward older MRs.

Selection was made after consulting the existing `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`. The historical !17571-!17620 batch remains counted as reviewed. The corpus commit is unchanged from the preceding runs, so no newly populated higher-numbered corpus holes displaced this frontier.

Exactly 50 MRs were reviewed in this run:

`!19839, !19838, !19837, !19836, !19835, !19834, !19833, !19832, !19831, !19830, !19829, !19828, !19827, !19826, !19825, !19824, !19823, !19822, !19821, !19820, !19819, !19818, !19817, !19816, !19815, !19814, !19813, !19812, !19811, !19810, !19809, !19808, !19807, !19806, !19805, !19804, !19803, !19802, !19801, !19800, !19799, !19798, !19797, !19796, !19795, !19794, !19793, !19792, !19791, !19790`

## Review notes

The batch was predominantly maintenance, CI/build work, protocol-specific corrections, backports, and small cleanup changes. Merged MRs were weighted above closed/superseded work. No new durable convention was promoted merely to manufacture notebook churn when the evidence only corroborated rules already recorded.

### !19830 — tap-iostat: Fix a possible floating point exception crash

Deep review. Merged master change authored and merged by John Thacker. The bug came from treating a failed `g_strstr_len()` search as though it returned the terminating NUL pointer; subtraction from NULL could produce a large negative length that later participated in numeric processing. The accepted implementation uses `strpbrk()` to locate either decimal point or comma and `strcspn()` to count the fractional digits, with an unsigned intermediate matching the destination domain. This strongly corroborates existing notebook guidance that helper return/failure contracts must be honored exactly and that intermediate representation should match the semantic domain.

### !19787 — cpu_info: improve the CPU name when running under Rosetta 2

Deep review. Guy Harris-authored release-4.4 backport. The implementation detects Apple's `sysctl.proc_translated` and annotates CPU information with `Rosetta 2` so architecture-specific capabilities such as SSE 4.2 are not misleading when they are supplied by translation rather than the reported native CPU. Useful portability evidence, but as a backport with no substantive review discussion it did not justify a new general convention beyond existing platform/API guidance.

### !19838 / !19839 — PROFINET TSN-reference update sequence

!19838 was closed and superseded by the merged !19839. The merged successor was therefore weighted as the accepted evidence; neither supplied a durable coding or architecture lesson beyond keeping protocol terminology synchronized with current specifications.

### Other batch observations

!19831 is a focused UTC formatting follow-up fixing the zero-precision `Z` suffix path. !19820 updates Aruba vendor-specific subtype data from an external implementation. !19809/!19808/!19805/!19797 are CI/build-environment maintenance. !19792 removes a fuzzshark build-warning regression. !19790 is a narrow CMake correction. These and the remaining low-signal MRs were scanned for lifecycle, discussion, and implementation significance and did not warrant new notebook conventions.

## Notebook result

No separate convention file was changed in this run. The strongest findings independently corroborated existing helper-contract, type-domain, portability, and accepted-successor guidance; avoiding duplicate or over-specific rules keeps the notebook durable rather than turning it into an MR-by-MR changelog.
