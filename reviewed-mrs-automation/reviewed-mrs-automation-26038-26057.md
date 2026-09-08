# Supplemental Reviewed Wireshark Merge Requests — !26038–!26057

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit used for this batch: `2261458afc3e287c111ffe13f534d9aaf5af07af`

Exactly twenty MRs were reviewed in this batch: **!26038 through !26057 inclusive**.

| MR | State / target | Review depth | Durable result / weighting |
|---|---|---|---|
| !26038 | merged, master | Deep | John Thacker ESS fix: optional BIT STRING output can remain unset for zero-length input; initialize `VAL_PTR` destination to `NULL` and guard use. Promoted to `parser-edge-cases.md`. |
| !26039 | merged, master | Medium | Radiotap 0-length PSDU presentation correction. Accepted dissector/UI cleanup, but no new general convention beyond using specification-defined semantics in columns/field text. |
| !26040 | open, master | Scanned | WSLua proposal adding `TL_LIMIT_TO_DISPLAY_FILTER` exposure through `Listener.new()`. Down-weighted because it remains open/not approved; no implementation convention promoted. |
| !26041 | merged, master | Medium | Win32 CMake Lua discovery supports both vcpkg and legacy packaged layouts and finds PDBs. Useful build compatibility precedent; no new cross-project rule promoted. |
| !26042 | merged, master | Scanned | Extcap libssh logging-version adjustment because the upstream/backport boundary differed from earlier assumptions. Narrow compatibility maintenance; no durable new rule. |
| !26043 | merged, master | Medium | Consolidates common SSH extcap option/help plumbing and fixes drift among ciscodump/wifidump/etc. Reinforces centralizing shared option definitions; used as context for !26053, not promoted separately. |
| !26044 | closed, master | Discussion-focused | Kerberos oversized record-mark hardening proposal. Down-weighted because it closed unmerged. Gerald Combs explicitly requested `Fixes #21468` in the **commit message** so GitLab auto-closes the issue and release-note tooling can enumerate fixed bugs; Peter Wu also pointed to the documented backport workflow. Retained as authoritative submission/process guidance despite the unmerged implementation. |
| !26045 | merged, master | Deep | Synchrophasor configuration legitimately crosses TCP/UDP, so state is keyed by semantic addresses + protocol ID in a protocol-specific conversation type rather than a transport stream. Promoted to `parser-edge-cases.md`. |
| !26046 | merged, master | Deep | Peter Wu X.509 IF fix, approved/merged by John Thacker: nested malformed RDN re-entry can clear shared outer scratch state; outer use must revalidate it. Promoted to `parser-edge-cases.md`. |
| !26047 | merged, release-4.6 | Backport/corroboration | Backport of !26046. Confirms the nested-RDN/state fix; not counted as independent architectural evidence. |
| !26048 | merged, release-4.4 | Backport/corroboration | Backport of !26046. Same weighting as !26047. |
| !26049 | merged, release-4.6 | Backport/corroboration | Backport of !26038 ESS zero-length BIT STRING fix. Corroboration only. |
| !26050 | merged, release-4.4 | Backport/corroboration | Backport of !26038 ESS zero-length BIT STRING fix. Corroboration only. |
| !26051 | merged, master | Deep | John Thacker STUN fix: use version-defining length constraints for autodetection, but retain explicitly selected version and emit expert info for violations. Promoted to `parser-edge-cases.md`. |
| !26052 | merged, master | Scanned | Python tooling renames variables shadowing built-in `max` (and missed `min` cases). Sensible readability/static-analysis cleanup; too generic to promote as Wireshark-specific convention. |
| !26053 | merged, master | Deep | John Thacker shared SSH extcap host verification: verify server identity before client authentication; unknown keys require explicit update option, changed keys fail, GSS-authenticated key exchange is handled specially. Promoted to `extcap-security-conventions.md`. |
| !26054 | merged, master | Deep | Ronnie Sahlberg wiretap fix approved/merged by John Thacker: `peektagged_read_packet()` error paths must return `-1`, not boolean `false`/0, because the API has tri-state semantics. Promoted to `parser-edge-cases.md`. |
| !26055 | merged, master | Deep | John Thacker AgentX OSS-Fuzz hardening: create a payload subset tvb and make inner offsets relative to it, replacing overflow-prone header+payload end arithmetic. Promoted as strong corroboration in `parser-edge-cases.md`. |
| !26056 | merged, master | Deep | John Thacker CBOR OSS-Fuzz leak fix: do not allocate a composite tvb when the only candidate member has zero captured length and append will be a no-op. Promoted to `parser-edge-cases.md`. |
| !26057 | closed, master | Scanned | IDN audio/StageFeed changes closed shortly after submission with no substantive review. Down-weighted; no convention promoted. |

## Weighting notes

Merged master-branch MRs were weighted most heavily. Release backports !26047–!26050 are corroboration for their corresponding master fixes rather than independent evidence. Open !26040 and closed !26044/!26057 were not treated as accepted implementation architecture. The process comments in !26044 are retained separately because they are explicit maintainer guidance: Gerald Combs's `Fixes #...` commit-message requirement directly explains issue closure/release-note integration, and Peter Wu points contributors to the project's documented backport procedure.

No Guy Harris-authored or Guy Harris-reviewed MR in this twenty-MR batch supplied stronger contrary guidance. John Thacker's authored/approved/merged security and parser fixes therefore receive the highest implementation weight within this batch.

## Notebook changes from this batch

- `parser-edge-cases.md`: optional parser outputs; nested/re-entrant transient state; version autodetection vs explicit-version diagnostics; tri-state return contracts; subset-tvb payload boundaries; lazy composite-tvbuff allocation; cross-transport semantic conversation identity.
- `extcap-security-conventions.md`: SSH server identity verification before credential transmission and explicit known-host trust behavior.

Notebook commits created during this review:

- `a89e2a877e781d19222f97e30bee574be47687c1` — parser edge-case conventions.
- `0ef7eecf9f49852249b6f83f8dab05803a48c892` — extcap SSH security conventions.
