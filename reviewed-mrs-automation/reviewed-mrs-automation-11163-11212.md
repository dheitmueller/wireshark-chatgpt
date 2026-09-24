# Reviewed Wireshark merge requests: !11163–!11212

- **Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
- **Review direction:** descending MR number, newest available previously-unreviewed first
- **Exact MR count:** 50
- **Exact selected set:** !11212, !11211, !11210, !11209, !11208, !11207, !11206, !11205, !11204, !11203, !11202, !11201, !11200, !11199, !11198, !11197, !11196, !11195, !11194, !11193, !11192, !11191, !11190, !11189, !11188, !11187, !11186, !11185, !11184, !11183, !11182, !11181, !11180, !11179, !11178, !11177, !11176, !11175, !11174, !11173, !11172, !11171, !11170, !11169, !11168, !11167, !11166, !11165, !11164, !11163
- **Tracking consulted before selection:** `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run files under `reviewed-mrs-automation/`, including the immediately preceding `reviewed-mrs-automation-11213-11262.md` and the historical/high-number ledgers.
- **Historical tracking preserved:** the previously reviewed !17571–!17620 batch remains part of the already-reviewed set.
- **Prior frontier probe:** !11212 had only been inspected to establish the previous run's frontier and was not previously counted as reviewed.
- **Status mix:** 49 merged and 1 closed/unmerged (!11192).

Selection was made from the individually reconstructed already-reviewed set rather than by assuming any numeric interval was fully reviewed. Merged master changes are primary evidence. Stable-branch backports primarily corroborate accepted behavior. Closed, abandoned, and superseded proposals are down-weighted. Maintainer-authored or maintainer-approved changes—especially Guy Harris, John Thacker, Pascal Quantin, Gerald Combs, and other long-standing maintainers—receive correspondingly high weight.

## Exact review ledger

| MR | State | Review result |
|---|---|---|
| !11212 | merged | Qt explicit signal/slot cleanup for Go To Packet actions. Framework-specific maintenance; no new durable convention. |
| !11211 | merged | Qt Telephony menu actions moved to explicit connections. Framework-specific maintenance. |
| !11210 | merged | DCE/RPC DFS regenerated through PIDL; reinforces canonical generated-source workflow. |
| !11209 | merged | DCE/RPC conformance source updated to reproduce accepted generated encoding output; strong source-of-truth corroboration. |
| !11208 | merged | Deep/high weight. John Thacker adds PIDL regeneration to CI and fails when tracked generated output changes; promoted to `generated-code-conventions.md`. |
| !11207 | merged | PIDL-generated dissectors refreshed. Corroborates reproducible generation workflow. |
| !11206 | merged | release-4.0 backport of bounded iSCSI TargetAddress parsing; corroborates !11196. |
| !11205 | merged | Adds SVCCTL to PIDL build/regeneration targets. Generated-workflow maintenance. |
| !11204 | merged | Display-filter slices preserve typed string semantics while raw slices retain byte semantics. Useful type-system corroboration; no separate rule added. |
| !11203 | merged | DCE/RPC FRSRPC conformance source synchronized with generated result. Strong generated-source-of-truth corroboration. |
| !11202 | merged | NETLOGON alignment correction with capture-backed validation. Protocol-specific correctness. |
| !11201 | merged | SVCCTL target/build warning cleanup. Generated-build maintenance. |
| !11200 | merged | Lua tests now surface nested child output in failures. Good test-diagnostic practice; existing test guidance is sufficient. |
| !11199 | merged | Documentation whitespace cleanup. No durable convention. |
| !11198 | merged | USB DFU Apple-device quirk uses specific VID/PID registration rather than globally weakening class matching. Useful dispatch corroboration. |
| !11197 | merged | NETLOGON alignment correction with representative capture. Protocol-specific correctness. |
| !11196 | merged | Deep/high weight. John Thacker replaces raw C-string/pointer walking in iSCSI TargetAddress parsing with bounded TVBuff search/extraction; promoted to `tvbuff-parsing-conventions.md`. |
| !11195 | merged | SVCCTL EnumServicesStatusW response parsing improvement with capture and before/after evidence. Protocol-specific correctness. |
| !11194 | merged | release-4.0 backport of Diameter SMSF label fix. Corroboration only. |
| !11193 | merged | Diameter SMSF label correction. Naming correctness. |
| !11192 | closed | Down-weighted. Large Aruba UBT proposal closed with failed pipeline; review contains useful local style/API comments but is not accepted architectural precedent. |
| !11191 | merged | Normalizes ETSI ASN.1 source line endings to keep conversion/regeneration tooling reliable. Generated-source hygiene corroboration. |
| !11190 | merged | Qt Statistics menu explicit connection cleanup. Framework-specific maintenance. |
| !11189 | merged | Documentation sentence-fragment cleanup. No durable convention. |
| !11188 | merged | Qt Statistics menu explicit connection cleanup. Framework-specific maintenance. |
| !11187 | merged | Guy Harris-authored terminology/comment correction clarifying writecap supports pcap and pcapng. High-authority but narrow documentation correction. |
| !11186 | merged | Cryptography code migrates GLib/Windows-specific integer types toward standard C99 types. Existing portability/type guidance is sufficient. |
| !11185 | merged | Small field/TFS cleanup; review also reinforces whitespace and descriptive commit-message hygiene. No separate convention. |
| !11184 | merged | Deep. Clarifies streaming reassembly helper contract and `DESEGMENT_ONE_MORE_SEGMENT` semantics when the final missing length is unknown; promoted to `reassembly-conventions.md`. |
| !11183 | merged | ACDR SSH/SFTP handling notes that recorded payload is already decrypted/raw. Protocol-specific dispatch decision. |
| !11182 | merged | Large ASN.1/asn2wrs generated-code refresh removing static proto-value initialization. Generated-code maintenance; no additional cross-cutting rule beyond existing generator guidance. |
| !11181 | merged | release-3.6 backport of the Windows native error-channel fix from !11169. |
| !11180 | merged | release-4.0 backport of the Windows native error-channel fix from !11169. |
| !11179 | merged | WiMAX field registrations split/cleaned to satisfy static checks and remove duplicate/unused hf entries. Static-checker corroboration. |
| !11178 | merged | Static filter-name checker gets a narrow exception for legitimate `stat` names rather than broad disablement. Corroborates precise-checker-exception practice. |
| !11177 | merged | Removes a disabled duplicate hf entry; discussion notes both unnecessary conditional code and analysis scripts may need correction rather than accumulating false-positive suppressions. Corroboration only. |
| !11176 | merged | Deep/high weight. Pascal Quantin and John Thacker review NGAP Network Instance typing against the 3GPP octet-string contract; promoted to `field-value-semantics-conventions.md`. |
| !11175 | merged | Fixes stale/broken SMB2 `DEBUG_SMB2` build path. Useful reminder that optional/debug compilation paths can rot; no separate rule promoted. |
| !11174 | merged | Asterix generator/test adjustments land before the next scheduled upstream-spec refresh. Strong generated-automation corroboration, promoted with !11208. |
| !11173 | merged | Guy Harris-authored TShark `-G dissectors` registry-dump support. Useful introspection interface, but no additional convention needed. |
| !11172 | merged | USB BOS descriptor support. Protocol enhancement; no cross-cutting lesson. |
| !11171 | merged | Replaces manual `wmem_alloc0(sizeof(type))` patterns with typed `wmem_new0`. Style/helper corroboration. |
| !11170 | merged | HTTP/2 RFC 9218 setting update. Protocol maintenance. |
| !11169 | merged | Deep/extremely high weight. John Thacker uses `GetLastError()`/`win32strerror()` for the Windows capture error path; Guy Harris explicitly approved. Stable !11180/!11181 corroborate. Promoted to `platform-api-portability-conventions.md`. |
| !11168 | merged | Deep/high weight. John Thacker moves TShark's pipe/dissection compatibility check until tap-driven dissection state is known and before output preamble side effects. Promoted to `cli-option-interaction-conventions.md`. |
| !11167 | merged | Debug logging for TShark's derived dissection state is moved until after that state is computed. Corroborates !11168's phase-ordering rule. |
| !11166 | merged | Removes obsolete GTK-era recent settings and associated dead variables. UI cleanup. |
| !11165 | merged | UDS standard DID dissection support, approved/merged by John Thacker. Protocol-specific enhancement. |
| !11164 | merged | NetFlow preference adds CWR/ECE interpretation for one-byte TCP flags; maintainer review corrected reserved-bit presentation. Protocol-specific correctness. |
| !11163 | merged | Release-note entry for APN6 dissector support. Documentation-only. |

## Durable notebook updates

- **!11208 + !11209 + !11203 + !11210, corroborated by !11174 — make checked-in generated output a CI-enforced regeneration invariant.** Regenerate from canonical inputs in a cheap pre-merge stage, fail on repository drift, and repair source/generator inputs rather than hand-normalizing generated artifacts. Added to `generated-code-conventions.md` in commit `30b2a57668cc19ad5eeb1547c4035f79a73812b0`.
- **!11169 with stable !11180/!11181 — pair a failing platform API with its native error namespace/channel.** Windows `GetLastError()`/`win32strerror()` must be used for the corresponding Win32 failure path rather than stale/unrelated `errno`; Guy Harris explicitly approved the master fix. Added to `platform-api-portability-conventions.md` in commit `15e941ec7ed41993151b03807e334daf93cddace`.
- **!11196 with stable !11206 — keep delimiter search and packet-text extraction inside TVBuff bounds.** Avoid raw C-string/pointer walking for length-delimited packet text. Added to `tvbuff-parsing-conventions.md` in commit `fc3ec86ab18675ba85a13b75791ddbd23f0e16e8`.
- **!11176 — preserve octet-string typing when the specification permits binary and textual encodings.** Human-friendly printable presentation must not narrow the field's actual wire domain. Added to `field-value-semantics-conventions.md` in commit `b60ad01e458c610277d404ab26471dd1d26f5d80`.
- **!11168 + !11167 — validate mode-dependent CLI combinations after derived runtime state is known, but before output side effects.** Added to `cli-option-interaction-conventions.md` in commit `ba908a070cbbff55f0501846733d40742dcef530`.
- **!11184 — use `DESEGMENT_ONE_MORE_SEGMENT` when more stream data is required but the exact final deficit is not knowable.** Added to `reassembly-conventions.md` in commit `fd393b0f3d1bdadfe5eaa179f9a3c4c0c9fe8ba1`.

## Strong corroboration retained without duplicate notebook rules

- **!11200:** test failures should preserve child/harness stdout so CI diagnostics identify the actual failure instead of only reporting a wrapper assertion.
- **!11178 + !11179 + !11177:** static-analysis tools should model legitimate exceptions narrowly, while source should also be simplified when unnecessary conditional/duplicate registrations create avoidable checker noise.
- **!11175:** optional/debug compilation paths need periodic build coverage or they can accumulate stale names and syntax errors invisible to the normal configuration.
- **!11187:** terminology in capture-format APIs should distinguish pcap from the broader set of formats such as pcapng rather than using “libpcap” as an imprecise umbrella.

## Down-weighted evidence

- **!11192** was closed/unmerged with a failed pipeline. Its detailed reviewer comments are useful local evidence but are not treated as accepted architecture or submission precedent.

## Frontier check

MR **!11162** (`release-notes: Add update notes for the IPv6 dissector, which supports dissecting APN6 option now.`) exists in the same corpus commit and is closed/unmerged. It was inspected only to verify that the corpus continues below this batch; **it is not counted among these 50 reviewed MRs** and remains eligible for the next descending run.
