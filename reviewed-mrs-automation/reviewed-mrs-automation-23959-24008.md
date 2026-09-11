# Wireshark MR Review Automation: !23959–!24008

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection was made from the exact union of individual MR entries in `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers under `reviewed-mrs-automation/`. Numeric filename ranges were not treated as proof that every MR in a range was reviewed. The historical !17571–!17620 batch remains preserved and counted.

Exactly 50 previously unreviewed corpus MRs were reviewed in descending order. The exact set is recorded below.

| MR | Review status | Notes |
|---|---|---|
| !24008 | Deep/corroboration, merged | SBC decode-loop hardening keeps separate remaining input/output capacities and terminates on decoder error or zero progress; reinforces existing capacity/progress rules. |
| !24007 | Deep/corroboration, merged | Wiretap keeps `Buffer` logical length synchronized with writes; merged by Guy Harris. Reinforces existing Buffer-authority guidance. |
| !24006 | Deep, merged backport | Stable backport of deterministic test-environment fix from !23993. Promoted with !23993 to `test-environment-conventions.md`. |
| !24005 | Scanned, merged backport | Modbus RTU-over-TCP versus raw-RTU entry-point separation; corroborates context-specific dissection entry points. |
| !24004 | Deep/corroboration, merged backport | UDP RTP/T.38 conversations use strategy-aware conversation APIs; reinforces complete state/conversation identity. |
| !24003 | Scanned, merged | WTP recursive formatting moved from a fixed stack buffer to `wmem_strbuf_t`, removing a possible stack overflow. |
| !24002 | Deep/corroboration, merged | Modbus separates TCP and raw-RTU entry points so transport-specific validity rules are not incorrectly shared. |
| !24001 | Scanned, merged backport | Correct stable-branch resubmission of TLS EAP/PEAP conversation handling after bad-target !24000. |
| !24000 | Discussion-focused, closed | Wrong-target cherry-pick; Jaap Keuter flagged the target and author acknowledged. Down-weighted in favor of merged !24001. |
| !23999 | Scanned, merged | NAS 5GS v19.6.0 generated/specification update. |
| !23998 | Deep/corroboration, merged | PDCP LTE/NR verifies at least four captured bytes remain before subtracting digest length; reinforces checked length arithmetic. |
| !23997 | Scanned, merged | NR RRC v19.2.0 generated/specification update. |
| !23996 | Scanned, merged | LPP v19.2.0 generated/specification update. |
| !23995 | Scanned, merged | LTE RRC v19.2.0 generated/specification update. |
| !23994 | Discussion-focused, closed | Proposed cursor change for Qt info banner was rejected because it would hide the discoverability cue for a deliberately clickable widget; retained as negative UX evidence only. |
| !23993 | Deep, merged | Tests launching Wireshark tools must use the controlled `test_env` so user profiles/preferences cannot alter results. Promoted to `test-environment-conventions.md`. |
| !23992 | Discussion-focused, closed | Global heuristic-disable proposal was questioned by Michael Mann and not accepted; existing protocol-enable UI already covers the broad use case. |
| !23991 | Scanned, merged | QCDIAG additional RRC/NAS logcode support; protocol-specific. |
| !23990 | Scanned, open draft | Proposed parent/helper `pinfo` tracking optimization; unmerged draft, down-weighted. |
| !23989 | Deep/corroboration, merged | PRP length validation prevents underflow that affected MACsec padding; reinforces existing bounds/arithmetic rules. |
| !23988 | Discussion-focused, merged backport | Capture File Properties interface-name/description backport; Michael Mann explicitly questioned feature-vs-bugfix status and the regression justification was provided. |
| !23987 | Scanned, merged | IEEE 802.11 Aruba ARM IE field addition; protocol-specific. |
| !23986 | Scanned, merged | Documentation updates issue terminology to GitLab Work Items. |
| !23985 | Deep/corroboration, merged backport | Toshiba parser bounds grouped writes and reserves rounded-up capacity; release-4.4 backport of !23982. |
| !23984 | Deep/corroboration, merged backport | Guy Harris stable backport of Toshiba grouped-write/capacity fix; strong corroboration of existing buffer-capacity guidance. |
| !23983 | Scanned, merged | Windows Falco Rust-plugin packaging update. |
| !23982 | Deep/corroboration, merged | Toshiba hexdump parser only parses groups actually remaining and reserves capacity for the rounded write unit; reinforces existing capacity/bounds rules. |
| !23981 | Scanned, merged | Corrects wiretap Doxygen contract text; documentation accuracy. |
| !23980 | Deep/corroboration, merged | TRDP leak fix uses scope-owned `wmem_ascii_strdown`; reinforces allocator/ownership guidance. |
| !23979 | Scanned, merged | IEEE1905 IPv4 endianness fix and IPv6 field semantic rename. |
| !23978 | Scanned, merged | macOS Falco plugins switched to Rust implementations to avoid multiple-Go-plugin loading limitation. |
| !23977 | Scanned, merged | WSDG logging documentation clarification. |
| !23976 | Scanned, merged | Roon Discovery field addition and empirically corrected minimum length. |
| !23975 | Deep, merged | John Thacker removes an MR-only advisory reminder from merge trains and makes it interruptible so superseded pipelines do not consume capacity. Promoted to `ci-job-scope-conventions.md`. |
| !23974 | Deep/corroboration, merged | Defensive initialization for arrays passed to libgcrypt in response to Coverity findings; reinforces static-analysis guidance. |
| !23973 | Deep/corroboration, merged | `sscanf` failure handling checks `<= 0`, including EOF, rather than only zero; reinforces checking the complete API return contract. |
| !23972 | Scanned, merged | Restores historical IKEv2 names for now-obsolete attributes rather than labeling values that were once valid as merely reserved. |
| !23971 | Scanned, merged | Adds newly assigned CIP objects; protocol-data maintenance. |
| !23970 | Scanned, merged | Stratoshark NSIS packaging uses the Stratoshark profile manifest rather than Wireshark's. |
| !23969 | Deep/corroboration, merged backport | Release-4.6 backport of checked zlib/decompression and text-growth arithmetic from !23967. |
| !23968 | Scanned, merged | Windows packaging adds missing AMR-WB dependency handling. |
| !23967 | Deep/corroboration, merged | Zlib decompression and text growth use checked arithmetic and explicit output limits; reinforces existing decompression/arithmetic guidance. |
| !23966 | Scanned, merged | Adds initial Stratoshark Kubernetes Audit profile. |
| !23965 | Discussion-focused, merged | CMake option models separate Wireshark/Stratoshark installation folders without baking the policy directly into OS checks; useful build design evidence but not promoted separately. |
| !23964 | Discussion-focused, merged | Translation files added to CMake install; later John Thacker questioned duplication because translations are embedded resources. Retained as review context, not a new rule. |
| !23963 | Scanned, merged | NSIS installer adds missing codec libraries. |
| !23962 | Deep/corroboration, merged | John Thacker replaces raw `Buffer` array mutation with Buffer APIs that update `first_free`; strongly reinforces existing Buffer abstraction ownership. |
| !23961 | Scanned, merged | P4RPC whitespace/comment cleanup with no functional change. |
| !23960 | Deep/corroboration, merged | ICEP rejects impossible negative signed lengths before arithmetic and removes duplicate fetching; reinforces checked-domain and fetch-once guidance. |
| !23959 | Scanned, merged | Removes dead stores identified by Clang Analyzer in IEEE 802.11 dissector; static-analysis cleanup. |

## Durable notebook effect

- Added `test-environment-conventions.md`: merged master !23993 plus accepted stable backport !24006 establish that command-line integration tests should run with the suite-defined controlled environment rather than inheriting a developer's profile, disabled-protocol state, or preferences.
- Added `ci-job-scope-conventions.md`: merged !23975, authored and merged by John Thacker, establishes that purely MR-facing advisory jobs need not rerun in merge trains and should be interruptible when a newer commit makes their output stale.
- !24008, !24007, !23998, !23989, !23985/!23984/!23982, !23980, !23974, !23973, !23969/!23967, !23962, and !23960 primarily corroborate existing notebook rules on strict progress, authoritative Buffer state, capacity, checked arithmetic, ownership, static-analysis handling, and bounded decompression; duplicate rules were not added.
- Closed !24000, !23994, and !23992 and open draft !23990 were deliberately down-weighted. Their discussion was retained only where useful as negative or process evidence.
