# Supplemental Reviewed Wireshark Merge Requests — !26178–!26197

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the earlier files in `reviewed-mrs-automation/` when selecting unreviewed MRs.

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`2261458afc3e287c111ffe13f534d9aaf5af07af`**
- Exactly twenty previously unreviewed MRs were reviewed: **!26178 through !26197 inclusive**.
- No MR outside that range was counted as reviewed in this batch.

## Weighting

Merged master-branch implementation and substantive human-review evidence received the greatest weight. Stable-branch automatic updates and cherry-picks were recorded for ledger completeness but were treated as corroboration or low-information maintenance rather than independent architectural evidence. John Thacker authored, reviewed, approved, or merged many of the highest-value changes in this range and was weighted accordingly; Anders Broman's accepted implementations and approvals were also weighted strongly. No Guy Harris-authored or Guy Harris-reviewed MR appeared in this twenty-MR window, so no new Guy-specific convention was inferred.

| MR | State / target | Review depth | Durable finding or disposition |
|---|---|---|---|
| !26178 | merged, master | Medium / maintainer discussion | Extcap device sorting/null-handling fix, approved/merged by Anders Broman. John Thacker explicitly distinguished a nullable `device->name` (which `g_strcmp0()` already supports) from a potentially invalid/null `device` owner. Useful corroboration that nullability must be reasoned about at the exact object/member level, but existing memory/nullability guidance is sufficient. |
| !26179 | merged, master | Shallow / documentation | Adds etwdump release notes for additional ETW providers and capture-option/undecipherable-message improvements. Accepted by Anders Broman; documentation-only, no new coding convention. |
| !26180 | merged, master | Deep / high authority | Nettrace had used `INT_MAX` as an initial growable-buffer reservation, causing GLib to request roughly 2 GiB before parsing. Accepted fix reserves 64 KiB and grows normally. Promoted distinction between logical maximum and initial capacity to `memory-lifetime-conventions.md`. Approved/merged by John Thacker. |
| !26181 | merged, master | Deep / high authority | RTPS DATA_BATCH OOB write when preference value `0` meant an effective 1024-entry cap but disabled the raw-preference guard. Accepted code checks the normalized `sample_info_max`. Promoted normalized-effective-limit safety rule to `parser-edge-cases.md`. Approved/merged by John Thacker. |
| !26182 | merged, master | Deep / security + UI/CLI | Adds opt-in CSV spreadsheet-formula neutralization for tshark and GUI exports. CSV quoting alone does not make packet-controlled formula-looking values inert; the apostrophe is a spreadsheet text prefix rather than a paired quote. Default remains unchanged because neutralization alters exported values. Promoted to new `output-security-conventions.md`. |
| !26183 | merged, master | Deep / maintainer-authored | John Thacker adds `ws_socketpair()`: native `socketpair()` on POSIX and AF_UNIX-backed equivalent on supported Windows, with temporary-path lifecycle hidden in wsutil. Promoted together with !26189 as cross-platform readiness/wakeup architecture in `extcap-runtime-conventions.md`. |
| !26184 | merged, release-4.4 | Shallow / automatic maintenance | Automatic generated-number/help/version refresh for release-4.4, authored by Gerald Combs and merged by Anders Broman. Routine release maintenance; no independent convention promoted. |
| !26185 | merged, release-4.6 | Shallow / automatic maintenance | Automatic generated-number/help/version refresh for release-4.6. Routine maintenance, low information density. |
| !26186 | merged, master | Shallow-medium / automatic maintenance | Master automatic update of generated help snapshots, assigned-number tables, translations, and related data. Confirms project automation of generated registries/docs but adds no new review convention. |
| !26187 | merged, master | Deep / high authority | John Thacker extends Windows Debug/Release library discovery for LZ4, CpuInfo, and XXHash to avoid MSVC CRT mismatch warnings. Strong corroboration of the existing imported-target/per-configuration dependency rule in `build-conventions.md`; not duplicated. |
| !26188 | merged, master | Medium / maintainer-authored cleanup | John Thacker removes stale InterfaceToolbar reader-thread state and teardown code left unused after the asynchronous control-path redesign. Good cleanup practice but no distinct new durable rule beyond removing obsolete lifecycle state. |
| !26189 | merged, master | Deep / high authority | John Thacker switches ssh-base wakeup/select handling to `ws_socketpair()` for cross-platform support and closes the control-out descriptor during cleanup. Strong implementation follow-through to !26183 and the existing interruptible-remote-loop convention; incorporated into `extcap-runtime-conventions.md`. |
| !26190 | merged, master | Medium-high / authoritative docs | Jaap Keuter updates developer documentation on conversation APIs, heuristic dissectors, and the example dissector while removing stale modelines; merged by Anders Broman. Valuable source-of-truth documentation maintenance, but the relevant heuristic/conversation guidance was already represented in the notebook, so no duplicate section was added. |
| !26191 | merged, master | Deep / highest robustness weight | John Thacker rejects TTL files whose global block size is smaller than an entry header and adds a cap on consecutive corrupt/alignment-recovery blocks. The MR documents that seek-only recovery may not set EOF, so malformed files could loop indefinitely. Promoted open-time structural-invariant and bounded-recovery guidance to `parser-edge-cases.md`. |
| !26192 | merged, release-4.6 | Shallow / backport | Cherry-pick of !26191 to release-4.6. Strong corroboration that the TTL robustness fix was stable-branch worthy, but not independent architectural evidence. |
| !26193 | merged, master | Medium / feature | Adds native Windows Bluetooth HCI capture to etwdump through the Microsoft BTHPORT ETW provider, plus release-note/provider plumbing. Approved/merged by Anders Broman; primarily feature-specific. |
| !26194 | merged, master | Deep / high authority CI | John Thacker fixes cppcheck report generation by testing for the exact XML artifact consumed by `cppcheck-htmlreport`, rather than a preceding text artifact that only conditionally produces XML. Promoted exact-consumer-artifact gating to new `ci-tooling-conventions.md`. |
| !26195 | merged, master | Deep / high authority extcap lifecycle | John Thacker sends `SP_QUIT` over extcap control channels, guards shared control-stream writes, and updates the Python example to stop its capture loop on quit. Promoted graceful protocol-level shutdown and serialized control writers to `extcap-runtime-conventions.md`. |
| !26196 | merged, master | Medium / maintainer-authored protocol fix | Anders Broman corrects GSM BSSMAP cell-ID discriminator 0x0c so decoding intentionally falls through to include CI, with specification comments for related cases. Protocol-specific accepted fix; no broad convention promoted. |
| !26197 | merged, master | Deep / high authority tooling | John Thacker documents cppcheck's counterintuitive stream contract (report on stderr, some parsing errors on stdout) and explicitly swaps streams so the report can be colorized/tee'd without hiding errors. Promoted tool-stream-contract guidance to `ci-tooling-conventions.md`. |

## Notebook updates promoted from this batch

- `memory-lifetime-conventions.md`: distinguish logical maximum/current length from initial capacity; growable parser buffers should start at a modest reservation instead of preallocating the theoretical maximum (!26180).
- `parser-edge-cases.md`: normalize preference sentinels once and apply all storage/loop bounds to the effective value (!26181); validate impossible file-wide structure at open time and bound recovery loops independently of EOF when iterations may only seek (!26191, corroborated by !26192).
- `extcap-runtime-conventions.md`: use a shared selectable-socket abstraction for cross-platform wakeups (!26183, !26189); prefer framed `SP_QUIT` graceful shutdown, serialize shared control writers, and keep reference extcaps current with lifecycle protocol changes (!26195).
- `output-security-conventions.md`: CSV syntax escaping does not neutralize spreadsheet formulas; packet-controlled exports may need consumer-specific neutralization, balanced against output-compatibility semantics (!26182).
- `ci-tooling-conventions.md`: gate downstream optional stages on their exact consumed artifact (!26194), and route tool streams according to the tool's actual stdout/stderr contract without suppressing diagnostics (!26197).

CMake !26187, documentation !26190, and several straightforward feature/maintenance MRs were intentionally retained as corroboration or ledger-only evidence rather than duplicating established notebook guidance.
