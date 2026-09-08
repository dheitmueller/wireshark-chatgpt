# Supplemental Reviewed Wireshark Merge Requests — !26018–!26037

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit used for this batch: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

Exactly twenty previously unreviewed MRs were reviewed in this batch: !26018 through !26037 inclusive.

| MR | Status | Review depth | Notes |
| --- | --- | --- | --- |
| !26018 | merged, release-4.6 | Corroboration | Backport of the Android btsnoop unsigned-length/overflow validation already established by !26012. No independent new rule. |
| !26019 | merged, release-4.4 | Corroboration | Second release backport of the Android btsnoop length hardening from !26012. |
| !26020 | merged, master | Deep | Guy Harris consolidated `pint.h` generated inline helpers so API signatures are defined once and private generation macros do not leak. John Thacker explicitly found the result easier to read and less mistake-prone. Promoted with very high weight. |
| !26021 | merged, master | Deep | Guy Harris established an explicit reader pattern for unsigned remaining-length counters: checked subtract before reads unless a prior proof makes the later subtract unconditionally safe. Promoted with very high weight. |
| !26022 | merged, release-4.4 | Scanned | Automatic registry/manufacturer/services/translations update. No durable engineering convention extracted. |
| !26023 | merged, release-4.6 | Scanned | Automatic registry/manufacturer/services/translations update. No durable engineering convention extracted. |
| !26024 | merged, master | Scanned | Automatic generated-data/documentation update across registries and encapsulation lists. No new durable rule. |
| !26025 | merged, master | Medium | Optional Plugin IF Demo needed C++ `override` annotations after broader Qt changes because the demo is not built by default. Useful reminder about optional targets, but not promoted as a standalone universal rule. |
| !26026 | merged, master | Deep | John Thacker validates exact 32-byte Curve25519 public/private key sizes before calling the crypto primitive, even when the malformed material comes from a Decryption Secrets Block. Promoted as an external-data/API-precondition rule. |
| !26027 | merged, master | Medium | John Thacker fixes an editcap error-path IDB-copy leak by unrefing on failure as well as success. Reinforces existing ownership/cleanup discipline; no separate new rule. |
| !26028 | merged, master | Deep | John Thacker hardens ERF writer alignment: truncated packets cannot be padded as if missing bytes were captured, and round-down must be rejected before underflow. Promoted as a writer-normalization rule. |
| !26029 | merged, release-4.4 | Corroboration | Backport of the BBlog TCPINFO minimum-option-length guard already captured from !26013. |
| !26030 | merged, master | Medium | John Thacker fixes NetMon open-failure leaks and uses the object destructor helper for nested process-info state rather than raw `g_free()`. Reinforces cleanup/ownership practice. |
| !26031 | merged, release-4.6 | Corroboration | Backport/co-authored Guy Harris cleanup making ERF byte-count arithmetic unsigned. Reinforces the existing semantic-domain rule for nonnegative sizes. |
| !26032 | merged, release-4.6 | Corroboration | Backport of !26028 ERF truncated-record alignment/underflow guard. |
| !26033 | merged, release-4.6 | Corroboration | Backport of !26026 Curve25519 exact-key-length validation. |
| !26034 | merged, release-4.4 | Corroboration | Second release backport of !26026 Curve25519 exact-key-length validation. |
| !26035 | merged, master | Discussion/medium | Lua 5.5/vcpkg transition. John Thacker identified a latent `LUA_API`/DLL-build assumption exposed by the packaging change. Useful dependency-integration evidence, but too implementation-specific for a new general notebook rule. |
| !26036 | merged, master | Deep | John Thacker rejects impossible variable snoop pseudo-header sizes with checked subtraction before derived allocation/read sizes can overflow into a near-4-GiB allocation. Promoted as a pre-allocation structural-validation rule. |
| !26037 | open draft, master | Discussion/scanned | Work-in-progress “Decode as C Escaped” UI feature. Guy Harris participated via commits, but there is no merged resolution and no substantive accepted-review conclusion in the corpus snapshot; implementation deliberately not promoted as project convention. |

## Durable notebook changes from this batch

`parser-api-conventions.md` now records five high-confidence conventions from this batch:

- macro-generated low-level APIs should have one canonical signature and should clean up private generation macros (!26020);
- remaining-length counters should be consumed using checked subtraction or an already-proven safe subtraction (!26021);
- exact crypto-library size preconditions must be checked when external data crosses the API boundary (!26026, corroborated by !26033/!26034);
- writer normalization/alignment must neither fabricate captured payload nor underflow semantic lengths (!26028, corroborated by !26032);
- malformed variable headers must be rejected before derived arithmetic can influence allocation or reads (!26036).

## Weighting notes

Merged master changes were weighted most heavily. !26020 and !26021 received especially high evidentiary weight because they were authored by Guy Harris and accepted after John Thacker review; !26020 also directly follows the width mismatch that caused !25994. John Thacker-authored security fixes !26026, !26028, and !26036 were also treated as high-confidence accepted practice. Release backports were used primarily as corroboration rather than counted as independent conventions. Automatic-update MRs were scanned but not mined for general rules. Open draft !26037 was down-weighted and recorded without promoting its implementation as accepted architecture.