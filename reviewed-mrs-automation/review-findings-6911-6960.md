# Wireshark MR review findings: !6911–!6960

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are weighted more heavily than closed, superseded, or still-open work. Maintainer-authored changes and direct maintainer review are called out where they materially strengthen the conclusion.

| MR | Outcome | Depth | Review finding |
|---|---|---|---|
| !6960 | closed | Discussion-focused | Second ZCL frame-type submission. Alexis La Goutte requested a rebase after the target advanced. Closed work is not implementation precedent; useful only as continuation of !6953 submission history. |
| !6959 | merged | Scanned | Large SDP extract-method refactor split media-attribute parsing into dedicated helpers. No substantive human review in the corpus snapshot; useful maintainability example but no new durable rule. |
| !6958 | merged | Discussion-focused | F5 trailer TLS 1.3 support. Alexis explicitly raised supported-branch backports after master acceptance; corroborates master-first/backport workflow. |
| !6957 | merged | Deep / high-authority workflow | Qt crash fix for unavailable ICU codecs. Alexis La Goutte and Gerald Combs explicitly required fixing master first and then cherry-picking to supported release branches; Gerald subsequently applied master and 3.4 follow-ups. Also shows that enumerating a nominal capability does not guarantee object construction succeeds. |
| !6956 | merged | Scanned | Debian packaging documentation corrected the actual packaging/debian path and symlink prerequisite. Documentation/build plumbing only. |
| !6955 | merged | Scanned | Packet Diagram explicitly updates the viewport after scene/root replacement to prevent stale visual residues. Qt-specific repaint fix with no broader maintainer discussion. |
| !6954 | merged | Scanned | release-3.6 form of the path-selection editor fix; condensed backport of master work represented by !6929. |
| !6953 | closed | Discussion-focused | First ZCL submission used the fork master branch. Alexis La Goutte instructed the contributor to close/reopen from a separate branch; continued as !6960. Strong workflow corroboration, but unmerged code is not implementation precedent. |
| !6952 | merged | Scanned | ISUP avoids duplicating parameter names in appended summary text. Local presentation cleanup. |
| !6951 | merged | Scanned | Documentation example supplied the missing foreground color entries. Documentation-only. |
| !6950 | merged | Scanned | release-3.6 ASTERIX generated-dissector/spec update. Generated-source workflow is already represented elsewhere; no additional review discussion. |
| !6949 | merged | Scanned | Corrected PathSelectionEdit member initialization/order warnings. Small compiler-cleanliness follow-up. |
| !6948 | merged | Scanned | clang-check validation skips source files that have no build rule in the active configuration and the Windows-only file_util.c where unavailable. Static-analysis tooling should respect the configured build/platform rather than report impossible translation units. |
| !6947 | merged | Scanned | Windows packaging CI switched the Qt bundle version and updated release notes. Dependency maintenance. |
| !6946 | merged | Scanned | Resolved Addresses dialog batches filter-column setup before invalidation and avoids redundant work. Useful local model-performance cleanup. |
| !6945 | merged | Scanned | release-3.6 backport of !6944 Windows extcap close handling; no additional lesson. |
| !6944 | merged | Scanned | Windows extcap pipe shutdown avoids invoking CRT close handling on an already-closed descriptor. Platform-specific lifecycle hardening. |
| !6943 | merged | Scanned | Automatic registry/data update. No reusable review lesson. |
| !6942 | merged | Scanned | Automatic registry/data update. No reusable review lesson. |
| !6941 | merged | Scanned | Automatic documentation/data update. No reusable review lesson. |
| !6940 | merged | Deep | John Thacker makes proto_tree_add_bitmask title construction honor BASE_SPECIAL_VALS and unit-string modifiers consistently. Special-value matches use their mapped text; unmatched values retain numeric fallback rather than being mislabeled Unknown. |
| !6939 | merged | Deep | John Thacker fixes unit-string rendering for 64-bit fields in bitmask titles, matching 32-bit field behavior. Corroborates consistent display-policy handling across width variants. |
| !6938 | merged | Deep | John Thacker fixes the reversed BASE_UNIT_STRING test for signed integer bitmask fields. Corroborates the same field-display contract. |
| !6937 | merged | Deep | John Thacker adds BASE_SPECIAL_VALS semantics to masked-field label generation, matching the non-bitmask path. A dissector had already tried to use the flag but the core silently ignored it, demonstrating why display modifiers must have uniform semantics across helper families. |
| !6936 | merged | Deep | Gerald Combs corrects many real conversation callers: find_conversation uses NO_ADDR_B/NO_PORT_B search wildcards while conversation_new uses NO_ADDR2/NO_PORT2 stored-endpoint omissions. This is earlier merged core evidence for the distinction later hardened by !7064 and caller follow-ups. |
| !6935 | merged | Scanned | Windows development artifacts moved to the new repository/domain and documentation/setup scripts changed together. Build-infrastructure migration. |
| !6934 | merged | Discussion-focused | Removed 32-bit support from win-setup after an earlier fatal-disable period; Gerald explicitly considered waiting a month after the fatal error before removing support. Useful staged-removal context, but policy-specific. |
| !6933 | merged | Scanned | Wiretap merge tempfile mode stops confusing a null tempdir with stdout output mode. API-argument semantics cleanup. |
| !6932 | merged | Scanned | Removed unused fourth DFVM instruction argument. Internal simplification. |
| !6931 | merged | Scanned | Removed the stale DFVM comment describing the deleted fourth argument. Documentation cleanup following !6932. |
| !6930 | merged | Deep | João Valverde answers a maybe-uninitialized warning by asserting the supposedly impossible enum case instead of inventing a default value. For code-controlled exhaustive state, make the invariant explicit; do not hide a missing enum case with meaningless initialization. |
| !6929 | merged | Deep | Reworked path editing into a reusable widget/delegate. John Thacker caught that committing/validating every keystroke could disrupt an active UAT editor because model/view refresh tears down the editor; review distinguishes transient text editing from commit-time validation. |
| !6928 | merged | Scanned | Added User Guide documentation for automatic updates. Documentation-only. |
| !6927 | merged | Deep | Gerald Combs fixes clazy incorrect-emit warnings: begin/end model operations are methods and should be called directly, while dataChanged is an actual signal and should be emitted. Preserve Qt's method-versus-signal API semantics rather than treating emit as decoration. |
| !6926 | merged | Scanned | Initializes MySQL lenstr after !6924 exposed a path where it could be used without assignment. Small correctness follow-up. |
| !6925 | merged | Scanned | Removed executable bits from C source files. Repository hygiene only. |
| !6924 | merged | Scanned | Corrected MySQL OK-packet response/message length handling and bounded it to remaining reported bytes. Follow-up !6926 completed initialization safety. |
| !6923 | merged | Deep / high-authority review | John Thacker switches text2pcap default output to pcapng. Guy Harris and Pascal Quantin explicitly discuss script compatibility: keep the old -n spelling accepted as a deprecated no-op/warning during transition, and avoid an unrelated binary rename that would break scripts. Docs and release notes change with behavior. |
| !6922 | merged | Deep | John Thacker makes mergecap discover and write pcapng IDBs that appear after packet processing begins, updating per-input interface maps as metadata arrives. The default all-IDB policy cannot be implemented exactly in one pass for future IDBs, so the accepted deterministic approximation is explicitly documented. |
| !6921 | merged | Scanned | Correct Qt API version threshold is 5.14 rather than 6.0. Capability checks should use the version that introduced the API, not a convenient major-version boundary. |
| !6920 | merged | Scanned | Npcap bundle upgrade with hashes, installer metadata, and release notes updated together. Dependency maintenance. |
| !6919 | merged | Scanned | AsciiDoc formatting and Windows named-pipe syntax corrections. Documentation-only. |
| !6918 | open/draft | Discussion-focused / low weight | Gerald Combs explored resurrecting old conversation wildcard behavior but explicitly left the MR draft because the existing behavior had been stable for almost 14 years and regressions were uncertain. Do not treat the proposed implementation as accepted conversation architecture. |
| !6917 | merged | Scanned | Version bump 3.7.0 to 3.7.1. Release mechanics only. |
| !6916 | merged | Scanned | Packaging export tolerates git stash create producing no stash/return failure and reports tarball reuse. Packaging robustness. |
| !6915 | merged | Scanned | 3.7.0 release-build/release-note preparation. Release mechanics only. |
| !6914 | merged | Scanned | Companion/backport of the Qt version-threshold correction represented by !6921. No additional lesson. |
| !6913 | merged | Scanned | release-3.6 backport of display-filter persistence fix represented by !6912. |
| !6912 | merged | Deep | Display-filter read/write becomes tolerant of platform line endings/formatting and serializes consistently. Persisted configuration parsers should not depend on one platform's exact textual formatting. |
| !6911 | merged | Deep | John Thacker moves DVB-S2 low-rolloff state from a file-global flag to conversation proto-data because multiple streams can coexist. It stores the transition frame, not merely a boolean, so redissection of earlier frames still uses pre-transition semantics. |

## Highest-value durable conclusions

1. Conversation lookup wildcards and conversation-creation omissions are different flag domains (!6936; later independently hardened by !7064 and follow-ups).
2. Stream-specific latched state belongs in conversation proto-data, and a transition frame/history is needed when redissection can revisit packets before the state change (!6911).
3. Wiretap metadata can arrive mid-stream; one-pass consumers must update interface maps at the record frontier and document any policy that cannot be implemented with future knowledge (!6922).
4. CLI names/options are compatibility surface. During a default change, preserve an obsolete spelling as a deprecated accepted form when practical, update public documentation, and avoid unrelated renames that unnecessarily break scripts (!6923; Guy Harris, Pascal Quantin, John Thacker).
5. Core field-display modifiers must retain the same semantics across ordinary fields, masked fields, bitmask title composition, signedness, and width variants (!6937–!6940, John Thacker).
6. Stable-branch bug fixes should normally be fixed on master first and then cherry-picked/backported (!6957, direct Gerald Combs and Alexis La Goutte guidance; corroborated by !6958).
7. A static-analysis warning about an exhaustive code-controlled enum is better answered by an explicit unreachable invariant than by dummy initialization that can mask a missing case (!6930).
8. Closed !6953/!6960 and open draft !6918 were deliberately down-weighted; their implementation choices are not current precedent.
