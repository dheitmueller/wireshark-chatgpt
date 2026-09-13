# Wireshark MR automation review: !20939-!20988

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, preserving and counting the historical !17571-!17620 batch. The fifty highest-numbered corpus MRs not already represented in that tracking were selected individually. All IDs !20939 through !20988 exist in this corpus snapshot, so this run contains exactly fifty MRs and has no corpus gaps.

Weighting: merged master changes are primary evidence; closed/draft/superseded work is down-weighted. Senior-maintainer-authored or reviewed changes, especially Guy Harris and John Thacker, receive correspondingly high weight.

| MR | Review weight | Result |
|---|---|---|
| !20988 | Scanned | Merged. Wiretap include/dependency cleanup; no new durable convention beyond existing header/dependency guidance. |
| !20987 | Scanned | Merged. O-RAN sequence analysis made direction-aware and links missing-sequence analysis to prior frames; protocol-specific state refinement. |
| !20986 | Scanned | Merged. Cilium DSR IP option support with sample capture; corroborates existing capture-evidence practice. |
| !20985 | Scanned | Merged. NGAP/HTTP2/GTP IMSI-to-TEID association; useful state integration but no sufficiently general new rule promoted. |
| !20984 | Discussion-focused | Merged after Michael Mann requested squashing logically inseparable commits to avoid unnecessary git-log noise. Useful submission-history evidence, but not promoted as a standalone hard rule. |
| !20983 | Deep | Merged, authored/merged by John Thacker. DTLS gets its own explicit stream identity for Follow Stream; corroborates the existing protocol-layer stream-identity convention. |
| !20982 | Scanned | Merged. Removes forgotten HTTP/2 debug prints; cleanup only. |
| !20981 | Scanned | Merged, authored by John Thacker. Local formatting consistency cleanup. |
| !20980 | Deep | Merged, authored by John Thacker. Supplies a portable Windows `SSIZE_MAX`; paired with !20978's signed-count contract. Added as supporting evidence to C API call-contract guidance. |
| !20979 | Scanned | Merged. HTTP/2 `:status` handling; protocol-specific functionality. |
| !20978 | Deep | Merged, authored by Guy Harris. Constrains sync-pipe read sizes to the signed result domain, checks `-1` before unsigned conversion/comparison, and motivated the Windows bound follow-up. Added to `c-api-call-contract-conventions.md`. |
| !20977 | Scanned | Merged. Improves display/filterability of unknown IP options; no new general convention. |
| !20976 | Scanned | Merged. Bluetooth HCI declaration/ordering cleanup; no durable new rule. |
| !20975 | Scanned | Merged. Wiretap internal-header Doxygen documentation. |
| !20974 | Scanned | Merged. Internet-address header Doxygen documentation. |
| !20973 | Scanned | Merged. Const qualification/static-analysis cleanup; corroborates existing const/static-analysis guidance. |
| !20972 | Scanned | Merged. Uses efficient JSON array iteration after shared iterator exposure; helper-reuse/performance cleanup. |
| !20971 | Scanned | Merged. API-reference build no longer runs by default; build-system-specific. |
| !20970 | Scanned | Merged. Wireless channel-width capability masking; protocol/platform-specific. |
| !20969 | Scanned | Merged. Parses HE capabilities even when EHT capability attributes are absent; platform-specific compatibility fix. |
| !20968 | Scanned | Merged. Removes obsolete GitLab CI setup; no durable new convention. |
| !20967 | Scanned | Merged. Windows CI switches Qt installation mechanism; tooling-specific. |
| !20966 | Scanned | Merged. Comment typo fix only. |
| !20965 | Scanned | Merged, Martin Mathieson. O-RAN tap output uses bounded buffers more carefully; corroborates existing buffer-sizing/bounded-output guidance. |
| !20964 | Scanned | Merged. Exposes shared JSON iteration helper used by !20972; no additional architecture rule beyond shared-helper reuse. |
| !20963 | Discussion-focused (closed/draft) | Closed draft concerning broken Diameter Custom.xml. Down-weighted and not treated as accepted implementation precedent. |
| !20962 | Scanned | Merged. Corrects IEC-prefix unit formatting; narrowly scoped formatting correctness. |
| !20961 | Deep | Merged, authored/merged by John Thacker. Preserves actual `-errno` values and snapshots them before cleanup can alter the error state. Added to `c-api-call-contract-conventions.md`. |
| !20960 | Scanned | Merged. Release-note cleanup only. |
| !20959 | Scanned | Merged. Falco Bridge adds typed address fields; no new general convention. |
| !20958 | Deep | Merged, authored and merged by Guy Harris. Builds DAAP tree-item summary information incrementally so valid earlier fields remain visible when a later TVB fetch throws on truncation. Added to `dissector-resilience-conventions.md`. |
| !20957 | Scanned | Merged. Replaces hard-coded Ethernet frame lengths with a shared macro; straightforward constant reuse. |
| !20956 | Scanned | Merged. O-RAN per-symbol transmission-duration tracking; protocol-specific state/analysis. |
| !20955 | Scanned | Merged. Falco JSON dissection uses a subset TVB rather than an unnecessary separate data source; corroborates existing bounded/subset-TVBuff guidance. |
| !20954 | Scanned | Merged. RTPS AES-128 PSK key-length correction with captures; corroborates focused reproducer/capture testing. |
| !20953 | Deep | Merged, authored by John Thacker and merged by Michael Mann. Checks `G_IO_STATUS_ERROR` as the authoritative failure indication even when GLib supplies no `GError`. Added to `c-api-call-contract-conventions.md`. |
| !20952 | Scanned | Merged. Coverity-oriented GLib allocation cleanup/annotation handling; no distinct durable rule promoted. |
| !20951 | Deep | Merged, authored/merged by Martin Mathieson. Replaces a packet-content-driven dissector assertion with expert information while investigating fuzz failures. Recorded in `dissector-resilience-conventions.md` and corroborates existing assertion guidance. |
| !20950 | Scanned | Merged. Clang Analyzer dead-assignment/increment cleanup; static-analysis maintenance only. |
| !20949 | Scanned | Merged. Adds Chrome NetLog Wiretap support; large format feature but no new cross-cutting review convention identified. |
| !20948 | Scanned | Merged. macOS artifact/Sparkle update; build tooling-specific. |
| !20947 | Scanned | Merged. O-RAN fuzz-error fixes; corroborates fuzz-driven hardening without a new distinct rule. |
| !20946 | Deep | Merged after extended architecture discussion. Moves `value_string` functionality to `wsutil`; important accepted layering change and precursor to later stable-branch dependency lessons already represented elsewhere in the notebook. |
| !20945 | Scanned | Merged. Removes obsolete libnl 1/2 support; dependency-support cleanup. |
| !20944 | Scanned | Merged. Documentation clarification that the Wiretap example is fictional. |
| !20943 | Deep | Merged. Removes ambient `wmem_packet_scope()` and directs dissectors to explicit `pinfo->pool`; strong corroboration of the existing narrow/explicit packet-scope allocator convention. |
| !20942 | Scanned | Merged. Wiretap header typo fix. |
| !20941 | Scanned | Merged. Const-qualifies read-only protocol data so it can reside in read-only storage; corroborates const-correctness guidance. |
| !20940 | Scanned | Merged. Migrates integer accesses to shared `wsutil/pint.h` helpers and unsigned domains; corroborates shared-helper/API-domain guidance. |
| !20939 | Deep | Merged, authored by John Thacker and merged by Anders Broman. Regularizes `ws80211_utils` error domains, stops utility code from unexpectedly printing libnl errors to stderr, and exposes explicit error-message retrieval. Added to `c-api-call-contract-conventions.md`. |

## Durable notebook changes from this run

- `c-api-call-contract-conventions.md`
  - The primary API status is authoritative even when an optional diagnostic object is absent (!20953).
  - Preserve one caller-visible error domain, snapshot volatile `errno` at the failure site, and avoid unexpected direct stderr output from utility APIs (!20939, !20961).
  - For signed count returns, constrain requests to the representable domain, test negative sentinels first, then convert successful counts to unsigned sizes (!20978, !20980).
- `dissector-resilience-conventions.md`
  - Publish successfully decoded summary information incrementally before later exception-capable TVB accesses (!20958, Guy Harris).
  - Treat malformed packet content as expert-reportable input rather than an assertion condition (!20951).

No MR outside the fifty entries above was counted as reviewed by this run.
