# Supplemental Reviewed Wireshark Merge Requests — !25998–!26017

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit used for this batch: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

Exactly twenty MRs were reviewed in this batch: !25998 through !26017 inclusive.

| MR | Status | Review depth | Notes |
| --- | --- | --- | --- |
| !25998 | merged, master | Deep | John Thacker moved CMS mutable tree pointers from globals into packet-private state, preventing stale proto-tree reuse across frames. Promoted as a packet-lifetime state rule. |
| !25999 | merged, master | Medium | Guy Harris documented that portable bytewise shift/mask integer-access idioms are optimized by newer GCC/Clang; promoted as evidence for portable helpers over alignment-sensitive tricks. |
| !26000 | merged, release-4.4 | Corroboration | Backport of the 3GPP log complete-hex-pair loop-bound fix. Strengthens the existing fixed-unit decoder rule; no independent new rule. |
| !26001 | merged, release-4.6 | Corroboration | Backport of !25998 CMS packet-private state fix. |
| !26002 | merged, release-4.4 | Corroboration | Backport of !25998 CMS packet-private state fix. |
| !26003 | merged, master | Deep | OBEX/L2CAP request-response lookup used a direction-specific CID; changed to connection-stable `local_cid`. Promoted as a state-key identity rule. |
| !26004 | merged, master | Deep | John Thacker changed ngsniffer header helpers to receive the number of bytes actually read rather than the larger logical record remainder. Promoted as a backing-buffer bounds rule. |
| !26005 | merged, release-4.6 | Corroboration | Backport of !26004 actual-buffer-length fix. |
| !26006 | merged, release-4.4 | Corroboration | Backport of !26004 actual-buffer-length fix. |
| !26007 | merged, master | Deep | John Thacker replaced recursive JSON-token traversal with iteration to avoid attacker-controlled stack growth; Guy Harris explicitly tied recursion here to fuzz-test stack overflows. Promoted with very high weight. |
| !26008 | merged, master | Deep | NetLog now cheaply validates a small semantic JSON signature before full parsing while tolerating legal member reordering. Promoted as a file-recognition/DoS-hardening rule. |
| !26009 | merged, release-4.6 | Corroboration | Backport of !26007 iterative JSON traversal. |
| !26010 | merged, release-4.4 | Corroboration | Backport of !26007 iterative JSON traversal. |
| !26011 | merged, master | Discussion/medium | Zigbee R23/Direct expansion. Review surfaced an `ett_` registration checker failure and subsequent checker correction. Useful CI evidence, but existing notebook structural-check guidance already covers the durable lesson. |
| !26012 | merged, master | Deep | John Thacker decoded btsnoop lengths as unsigned, validated capacity/ordering/overflow constraints before arithmetic, and only then used them for buffering/dumping. Promoted as a hostile-length rule. |
| !26013 | merged, master | Medium-deep | BBlog TCPINFO option validates its mandatory two-`uint64_t` minimum before accessing fields. Promoted as corroboration for local structure minimum checks. |
| !26014 | merged, master | Discussion/medium | Cell-broadcast language prefix decoded separately from message text. Pascal Quantin considered a proposed regression test unnecessary for this change and preferred the focused final commit. Recorded narrowly; not generalized into an anti-test rule. |
| !26015 | merged, master | Scanned/discussion | Broad Ruff cleanup over Python tooling with review corrections. No new durable rule beyond existing project-CI/Ruff and review-hygiene guidance. |
| !26016 | merged, release-4.6 | Corroboration | Backport of !26013 BBlog truncated-option guard. |
| !26017 | merged, master | Deep | Gerald Combs expanded `SECURITY.md`: concise confidential reports, reproduction steps, strongly recommended reproducing captures, and explicit note that attached captures feed fuzz testing. Promoted as security-reporting policy. |

## Durable notebook changes from this batch

- `parser-api-conventions.md`: packet-scoped mutable state; bidirectional state-key identity; actual-buffer versus logical-length bounds; iterative traversal of hostile nesting; cheap semantic file probes; unsigned wire-length validation before arithmetic; portable integer-access idioms; additional backport corroboration for fixed-unit decoding.
- `security-reporting.md`: official security-report formatting/reproducer guidance from !26017 and a narrowly scoped note on !26014's test-review discussion.

## Weighting notes

All twenty MRs in this batch were merged. Master-branch fixes were weighted more heavily than release backports. !25999 received elevated weight because it was authored and merged by Guy Harris. !26007 received especially high weight because its merged security fix is directly reinforced by Guy Harris's non-system review comment about fuzz-test stack overflows. Backports were used primarily as corroboration and were not counted as independent new conventions.