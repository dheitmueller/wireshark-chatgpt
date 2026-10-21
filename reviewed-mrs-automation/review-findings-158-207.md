# Review findings: MRs !158-!207

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed MRs were examined, newest to oldest. Merged master MRs were treated as strongest implementation evidence; stable-branch backports mainly corroborate their master changes. Closed !182, !181, and !175 were down-weighted. High-authority maintainer review, particularly from Guy Harris and Pascal Quantin, received additional weight.

## Highest-value findings

### !177 — decode the wire representation truthfully

The MBIM 2.0 work received detailed Pascal Quantin review. A version field was little-endian with an implied decimal notation, but the proposed implementation used big-endian decoding because current bytes happened to produce convenient values such as 1 and 2. Pascal rejected that: decode with the specified little-endian semantics and render the dotted version separately. His explicit future-value example shows why this matters: an accidental representation trick that works for current values will fail when the value domain expands. The same review also corrects a reserved-bit mask and recommends standard unit strings for dBm/dB presentation.

### !184 — machine-readable output changes need migration documentation

Follow Stream YAML was intentionally redesigned to carry peer records, packet records, timestamps, and per-peer indexes. Review accepted changing the development-branch schema but required the old form to be replaced cleanly, a release-note notice, user-guide documentation, and an explicit mapping from old keys to the new peer/index representation. The MR also exposed a portability problem in formatting `time_t`: a format that compiled on Windows failed on macOS, leading review toward a known-width cast plus the matching portable format macro.

### !180 — console bootstrap can corrupt redirected capture I/O

Moving Windows console/log initialization earlier made startup diagnostics available sooner, but post-merge discussion demonstrated that parent-console attachment could disrupt an inherited stdin pipe used for capture-from-stdin. The proposed correction preserves the original standard-input handle unless stdin actually needs console redirection, and the reporter confirmed the pipe worked again. The durable lesson is stronger than the first merged implementation: platform bootstrap must preserve redirected standard handles as resources.

### !166 — reviewability and repository identity are part of submission correctness

This long-lived MQ MR contains two process lessons. First, Pascal Quantin noted that the source diff exceeded GitLab's render limit, making meaningful web review impossible; the contributor reduced the change and deferred more work. Second, the contributor's remote named `upstream` accidentally pointed to the same personal fork as `downstream`, so repeated rebases misleadingly said “up to date.” Correcting the upstream remote to the canonical repository exposed the real history and allowed the rebase.

### !169 — a masked Boolean needs a value in the mask's input domain

QUIC Key Phase was reduced to logical 0/1 and then passed to a tree API for a Boolean field whose mask occupied a higher bit. The accepted change realigns the true value to the registered bit position. This independently reinforces the rule that the registered mask should perform extraction exactly once and that a value-taking API's input domain is not automatically the same as the user-visible normalized value.

### !203 — special-value tables do not make unmatched numbers unknown

Pascal Quantin extends 64-bit field rendering so `BASE_SPECIAL_VALS` matches existing 32-bit behavior. A matching table entry receives the special text; an unmatched number remains numeric instead of being labeled `Unknown`. Display modifiers are semantic policy and should behave consistently across integer widths.

### !202 — once a duplicated bug is found, inspect all sibling decoders

The GSM RR spare-bit fix initially targeted one Channel Description path. Pascal Quantin identified the same error in Channel Description 2 and 3 and asked that all three be corrected. This is strong review evidence for searching cloned/sibling parser paths once one copy is proven wrong. The MR also repeats the established request to allow maintainer edits so core developers can rebase or make minor corrections.

### !161 — canonical-project-only runners need explicit CI scoping

Gerald Combs restricts the Windows MR job to the canonical project because it depends on a project-specific Windows runner/custom image that ordinary forks cannot use. The accepted GitLab rules express both merge-request context and project identity. Contributor forks should not be made to fail jobs whose environment they cannot obtain.

### !158 — submission metadata follows the active review system

Pascal Quantin asked the contributor to refresh Wireshark's current commit-message hook and remove Gerrit `Change-Id` trailers because GitLab no longer uses them. History changes should then receive fresh CI validation. The implementation also contains the master BSSMAP IPv6 current-offset fix later carried by !162-!164.

### Guy Harris string-field work: !207, !205, !204, !191

These MRs give early authoritative examples of choosing string field types from the wire contract: BPDU configuration names and AFP passwords with true fixed-width NUL padding use padded-string semantics; Aeron's error string, which is not NUL-terminated, is an ordinary string. !207's SAP interpretation is historical and is superseded semantically by Guy's later introduction of a distinct truncated-string type for fields where bytes after an early NUL are undefined.

## Per-MR accounting

| MR | Outcome | Review | Notes |
|---|---|---|---|
| !207 | merged | Deep | Guy Harris SAP string-type analysis; historical precursor to later truncated-string semantics. |
| !206 | merged | Discussion-focused | SMB2 compression flags; Alexis requested displaying reserved bits and author incorporated them. |
| !205 | merged | Deep | Guy Harris BPDU fixed-width NUL-padding correction. |
| !204 | merged | Deep | Guy Harris AFP password fixed-width NUL-padding correction and source-link cleanup. |
| !203 | merged | Deep | Pascal Quantin adds 64-bit `BASE_SPECIAL_VALS` behavior matching 32-bit rendering. |
| !202 | merged | Deep | GSM RR spare-bit fix; Pascal required correcting all duplicated sibling paths and maintainer-edit access. |
| !201 | merged | Discussion-focused | gQUIC MAD0 field; Pascal corrected type and suggested `proto_tree_add_item_ret_uint()`. |
| !200 | merged | Scanned | SMB2 negotiate-context additions from updated Microsoft specification. |
| !199 | merged | Scanned | master-3.0 CI backport adding MR jobs/cache setup. |
| !198 | merged | Scanned | master-3.0 backport of gQUIC CTIM little-endian time decoding. |
| !197 | merged | Scanned | master-3.2 backport of gQUIC CTIM little-endian time decoding. |
| !196 | merged | Scanned | MQ structure/display improvements and newer structure support. |
| !195 | merged | Scanned | gQUIC Q050/T050/T051 encoding corrected to big-endian. |
| !194 | merged | Scanned | Master gQUIC CTIM fix adds little-endian flag to time decoding. |
| !193 | merged | Scanned | MQ formatting-only cleanup. |
| !192 | merged | Scanned | Dissector spelling corrections. |
| !191 | merged | Deep | Guy Harris corrects Aeron Error String from NUL-terminated to ordinary string. |
| !190 | merged | Scanned | SDP accepts MCVideo as a non-numeric fmtp token. |
| !189 | merged | Scanned | Stable-branch README email-address update. |
| !188 | merged | Scanned | CI reduces log volume with silent make flags. |
| !187 | merged | Scanned | master-3.2 MR-CI backport; Guy confirmed the merge-pipeline repair. |
| !186 | merged | Scanned | master-3.0 backport migrating gen-bugnote from Bugzilla to GitLab API. |
| !185 | merged | Scanned | master-3.2 backport of gen-bugnote GitLab migration. |
| !184 | merged | Deep | Follow Stream YAML schema redesign, migration docs, release notes, cross-platform time-format issue. |
| !183 | merged | Scanned | Release-note spelling corrections. |
| !182 | closed | Scanned | Failed/superseded stable email-address attempt; GitLab pipeline-status trouble only. |
| !181 | closed | Scanned | Earlier failed/superseded stable email-address attempt. |
| !180 | merged | Deep | Earlier Windows console/log setup; discussion exposes redirected-stdin regression and preservation strategy. |
| !179 | merged | Discussion-focused | SIP RFC 8497 logme support; maintainer-edit workflow repeated. |
| !178 | merged | Scanned | Master gen-bugnote migration from Bugzilla to GitLab API. |
| !177 | merged | Deep | MBIM 2.0; Pascal review on true endianness, display formatting, masks, and units. |
| !176 | merged | Scanned | Master README email-address update by Guy Harris. |
| !175 | closed | Scanned | Empty/superseded precursor to !176. |
| !174 | merged | Scanned | Release-note cleanup and dissector-name correction. |
| !173 | merged | Scanned | README typo correction. |
| !172 | merged | Scanned | Feature-request template typo correction. |
| !171 | merged | Discussion-focused | Missing-prototype cleanup; discussion clarifies GitLab squash option behavior. |
| !170 | merged | Scanned | Qt unused assignment removed for Coverity finding. |
| !169 | merged | Deep | QUIC masked-Boolean Key Phase value aligned to registered mask. |
| !168 | merged | Scanned | Wireshark 3.0 EOL release-note preparation. |
| !167 | merged | Scanned | Wireshark 2.6 final-release/EOL notice. |
| !166 | merged | Deep | MQ 9.2; reviewability limit, correct upstream remote, rebase/maintainer workflow, warning cleanup. |
| !165 | merged | Discussion-focused | MySQL/MariaDB support; narrow checker exception permits MariaDB field prefix in shared dissector file. |
| !164 | merged | Scanned | master-2.6 backport of BSSMAP IPv6 current-offset fix. |
| !163 | merged | Scanned | master-3.0 backport of BSSMAP IPv6 current-offset fix. |
| !162 | merged | Scanned | master-3.2 backport of BSSMAP IPv6 current-offset fix. |
| !161 | merged | Deep | Gerald Combs scopes Windows MR CI to canonical project-specific runner. |
| !160 | merged | Scanned | Broad spelling cleanup and checker dictionary adjustments. |
| !159 | merged | Discussion-focused | PROFINET removes obsolete CBA-version gating after compatibility concern was challenged against current spec. |
| !158 | merged | Deep | Master BSSMAP fixes; Pascal guidance on current commit hook, obsolete Gerrit trailers, CI, and maintainer edits. |

No SMPTE ST 291/VANC packet type was encountered.
