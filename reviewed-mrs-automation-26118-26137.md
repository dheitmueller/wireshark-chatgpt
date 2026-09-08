# Supplemental Reviewed Wireshark Merge Requests — !26118–!26137

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

## Corpus provenance

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit used for this entire review batch: **`2261458afc3e287c111ffe13f534d9aaf5af07af`**
- Exactly twenty previously unreviewed MRs were reviewed: **!26118 through !26137 inclusive**.
- No MR outside that range was counted as reviewed in this batch.

## Weighting

Merged master-branch implementation and review evidence was given the greatest weight. Stable-branch release mechanics and cherry-picks were recorded for ledger completeness but treated primarily as corroboration. Maintainer-authored or maintainer-reviewed changes, particularly those from John Thacker, Martin Mathieson, Gerald Combs, and Anders Broman, were weighted more heavily than mechanically merged release work.

| MR | State / target | Review depth | Durable finding or disposition |
|---|---|---|---|
| !26118 | merged, master | Medium | Martin Mathieson cleanup of warnings reported by Wireshark checking scripts across dissectors. Mostly typo/unused-state/static-check cleanup; useful corroboration that checker warnings are expected to be addressed, but no distinct new convention promoted. |
| !26119 | merged, master | Deep | John Thacker Roughtime heuristic adds cheap structural rejection before claiming the protocol: require enough captured bytes, accept either the IETF magic or a plausible bounded Google-version outer-tag count, and reject otherwise. Tested against the existing !18559 capture. Strong corroboration of conservative heuristic recognition on unofficial/shared ports. |
| !26120 | merged, release-4.6 | Shallow | Gerald Combs release preparation for 4.6.8. Release mechanics only; no coding/architecture rule promoted. |
| !26121 | merged, release-4.4 | Shallow | Gerald Combs release preparation for 4.4.18. Release mechanics only; no coding/architecture rule promoted. |
| !26122 | merged, master | Deep | John Thacker CMake change, approved/merged by Gerald Combs. Adds an imported MaxMindDB target and separates Debug/Release artifacts on Windows to avoid MSVC CRT mismatches. Promoted: external dependency configuration and transitive requirements belong on imported targets. |
| !26123 | merged, master | Deep | John Thacker extcap/MSYS2 fix. Uses the correct `DWORD` format, follows GLib's cast-through-generic-function-pointer approach for a callback warning, and treats normal `ERROR_BROKEN_PIPE`/control-pipe closure as clean teardown rather than a user warning. Promoted extcap lifecycle rule. |
| !26124 | merged, master | Deep | John Thacker RTCP heuristic strengthening. Requires enough captured bytes to read the length field, then checks reported packet length is at least the protocol-declared first-packet length, deliberately allowing extra bytes for compound RTCP/SRTCP. Strong corroboration: enforce protocol lower bounds without rejecting valid extensions/compound forms. |
| !26125 | merged, master | Deep | SAP DIAG hardening by Martin Gallo with detailed Martin Mathieson review. Validates variable-length item headers/lengths, uses subset tvbs, bounds NUL searches with `tvb_strnlen()`, reports missing terminators, fixes an impossible negative check on unsigned `tvb_reported_length_remaining()`, and uses value-returning tree helpers where appropriate. Contributor also supplied a working fuzzing script. Promoted bounded-terminator rule; subset-tvb lesson corroborates existing notebook guidance. |
| !26126 | merged, master | Deep | SAP DIAG/RFC decompression by Martin Gallo, approved/merged by Anders Broman. Validates compression/reassembly metadata before access/allocation, caps uncompressed sizes, only decompresses complete logical units, exposes successful output as derived data, reports failures/mismatches with expert info, and preserves raw content when disabled/failing/incomplete. Representative SAP GUI capture supplied. Promoted bounded-decompression/raw-fallback rule. |
| !26127 | merged, master | Shallow | Gerald Combs libssh 0.12.2 dependency update. Dependency maintenance only; no distinct convention promoted. |
| !26128 | merged, release-4.6 | Shallow | Cherry-pick/backport of !26127 to release-4.6. Corroboration only; not double-counted as new architecture. |
| !26129 | merged, release-4.4 | Shallow | Cherry-pick/backport of the libssh update to release-4.4, with a FetchArtifacts conflict resolved. Corroboration/release maintenance only. |
| !26130 | merged, master | Medium | BLF FlexRay writer correctness fix changes flag construction from `&=` to `|=` so asserted frame properties are actually set. Clear bug fix, but the bitwise lesson is general C correctness rather than a Wireshark-specific durable convention, so retained only in ledger. |
| !26131 | merged, master | Deep | John Thacker sshdump lifecycle change, approved/merged by Anders Broman. Replaces an indefinitely blocking SSH read with readiness-based waiting and a non-Windows self-pipe so graceful shutdown wakes even when a capture filter yields zero packets; teardown can explicitly terminate the remote command. Promoted extcap interruptibility/shutdown rule. |
| !26132 | merged, master | Deep | John Thacker CMake Sinsp change, approved/merged by Gerald Combs. Models a multi-library external dependency as `INTERFACE IMPORTED`, attaching include directories, Debug/optimized library sets, and a narrowly scoped MSVC link warning suppression; consumers link the target instead of duplicating usage requirements. Promoted together with !26122. |
| !26133 | merged, release-4.6 | Shallow | Gerald Combs build/release commit for 4.6.8 (`[skip ci]`). Release mechanics only. |
| !26134 | merged, release-4.4 | Shallow | Gerald Combs build/release commit for 4.4.18 (`[skip ci]`). Release mechanics only. |
| !26135 | merged, release-4.6 | Shallow | Gerald Combs post-release version bump 4.6.8 → 4.6.9 (`[skip ci]`). Release mechanics only. |
| !26136 | merged, release-4.4 | Shallow | Gerald Combs post-release version bump 4.4.18 → 4.4.19 (`[skip ci]`). Release mechanics only. |
| !26137 | merged, release-4.6 | Shallow | Gerald Combs minor release-notes fix. Documentation/release maintenance only; no new durable convention promoted. |

## Notebook updates promoted from this batch

- `build-conventions.md`: imported targets for external dependencies, including per-configuration artifact selection, transitive usage requirements, and dependency-scoped link options (!26122, !26132).
- `parser-edge-cases.md`: bound terminator searches to enclosing fields (!26125); bound decompression/reassembly output and preserve raw bytes on disabled/failed/incomplete transformation (!26126).
- `extcap-runtime-conventions.md`: normal control-channel closure is not a user-facing error (!26123); remote-capture loops must remain interruptible even when no capture data arrives (!26131).

Heuristic lessons from !26119 and !26124 and subset-tvb handling from !26125 were intentionally not duplicated in durable files because the notebook already contains strong accepted guidance covering those patterns.