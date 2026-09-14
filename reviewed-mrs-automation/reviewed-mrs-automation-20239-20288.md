# Wireshark MR Review Batch — !20239 through !20288

Corpus revision reviewed: `dheitmueller/wireshark-corpus-mrs@9e52bc78659a888d4eb624984ee1a886a40d959f`

## Selection and de-duplication

Before selecting this batch, the already-reviewed set was rebuilt from all available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the supplemental automation tracking, and the per-run files under `reviewed-mrs-automation/`. Numeric ranges were not assumed complete merely because some MRs in them appeared in a ledger. The historical !17571–!17620 batch remains explicitly preserved and counted as reviewed.

The immediately preceding backward-review batch was !20289–!20338. Every corpus object from !20288 through !20239 exists at the pinned corpus revision and none was found in the already-reviewed set, so these are the next fifty highest-numbered previously unreviewed MRs.

## Exact reviewed set

!20288, !20287, !20286, !20285, !20284, !20283, !20282, !20281, !20280, !20279,
!20278, !20277, !20276, !20275, !20274, !20273, !20272, !20271, !20270, !20269,
!20268, !20267, !20266, !20265, !20264, !20263, !20262, !20261, !20260, !20259,
!20258, !20257, !20256, !20255, !20254, !20253, !20252, !20251, !20250, !20249,
!20248, !20247, !20246, !20245, !20244, !20243, !20242, !20241, !20240, !20239.

Count: **50**.

## Review weighting and durable findings

- **!20282 — Deep, merged master, very high weight.** `register_tap_listener()` had an error path that invoked the full tap-listener destructor even though initialization was incomplete and callers already retained cleanup responsibility on registration failure. That created inconsistent callback behavior and a potential double free. The accepted fix frees only the allocation that is known to exist at that stage. Promoted to `lifecycle-partial-initialization-conventions.md`.
- **!20285 — Deep, merged master, John Thacker-authored/merged.** A helper parsing a subset tvbuff starts at offset zero in the subset's own coordinate space, so its returned consumed length is already relative to that subset; subtracting the parent tvbuff's offset again is incorrect. Promoted to `tvbuff-coordinate-conventions.md`.
- **!20250 with release-4.4 backport !20263 and follow-up !20251 — Deep, merged, very high weight.** John Thacker corrected Androiddump's Windows socket error handling to use the Winsock return/error contract (`SOCKET_ERROR` plus `WSAGetLastError()`) rather than POSIX `errno`, then safely restored timeout behavior once errors were interpreted correctly. Promoted to `platform-error-contract-conventions.md`.
- **!20256 — Deep, merged master after extended review.** Pascal Quantin's ESP-SA DSB support evolved during review to export only SAs actually used by the capture, reducing unnecessary secret disclosure, and the new pcapng secrets type was taken to the external pcapng specification process. Added to `secrets-redissection-conventions.md` as guidance for capture-secret export minimization and format coordination.
- **!20257 — Deep, merged master, substantive Pascal Quantin and Guy Harris review.** Pcapng bytes after an end-of-options marker are surfaced as explicit extraneous data rather than silently ignored. Review pushed toward exposing the actual bytes (not merely a marker) so TShark and non-GUI consumers can inspect them, and Guy Harris pointed to adversarial/polyglot material as useful validation. Retained as strong parser-observability/testing evidence; no separate rule added because existing raw-byte/parser-boundary guidance already covers the principle.
- **!20268 — Deep, merged master, first contribution with detailed John Thacker review.** PostgreSQL logical-replication dissection was checked against protocol-version availability; reviewer discussion explicitly questioned whether newer-version fields needed direct `proto_version` guards or whether surrounding state already proved the required version. Retained as strong evidence that version-dependent fields need a demonstrable gating invariant, but not duplicated into a new notebook rule.
- **!20245 — Deep, merged master, John Thacker-authored.** SSH packet-length expert warnings were corrected for cipher modes whose packet-length field is not encrypted under the same framing rule. The accepted logic follows the actual mode-specific wire construction rather than mechanically applying a base RFC sentence outside its domain. Strong protocol-semantics evidence, but narrow enough to remain in the ledger.
- **!20242 — Deep, merged master after substantial review.** NHRP gained IPv6 address handling and related registration changes. Because the review history and resulting implementation were protocol-specific and evolved substantially from the initial proposal, it was not promoted as a generalized Ethertype precedent.
- **!20240 — Scanned/deep-authority, merged master, authored and merged by Guy Harris.** Corrects stale historical terminology in a dissector API comment: an implementation should not preserve an old “new-style”/“old-style” distinction once the API transition has long since become the single normal model. High-authority documentation hygiene, but not a separate engineering convention.
- **!20264 versus !20262 — supersession weighting.** !20262 was closed after a failed initial TETRA call-identifier correction; !20264 is the merged master correction, with !20276/!20277 as stable backports. The merged path is treated as the precedent and !20262 is deliberately down-weighted.
- **!20288 — Merged master, cautionary rather than promoted.** The HTTP/2 IMSI mapping change was inspected in detail but not promoted as an exemplar; merge status alone is not treated as proof that every local implementation technique is a durable convention.

The remaining MRs were inspected for state, purpose, diff scope and available discussions. They were predominantly localized dissector fixes, stable backports, generated/encoding warning cleanup, UI presentation adjustments, documentation/release-note maintenance, or small protocol additions without distinct cross-cutting reviewer guidance. They count as reviewed but did not justify new notebook rules.

## Notebook changes from this run

- Added `lifecycle-partial-initialization-conventions.md` from !20282.
- Added `tvbuff-coordinate-conventions.md` from !20285.
- Added `platform-error-contract-conventions.md` from !20250/!20251/!20263.
- Extended `secrets-redissection-conventions.md` with the used-secret minimization and external-format-registration guidance from !20256.
