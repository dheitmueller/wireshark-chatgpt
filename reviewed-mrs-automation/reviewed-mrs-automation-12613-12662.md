# Automated Wireshark MR review: !12613–!12662

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection was built by consulting the available per-run ledgers in `reviewed-mrs-automation/`, `reviewed-mrs.md`, and the supplemental automation tracker, then subtracting every explicitly reviewed MR number from the corpus. The historical !17571–!17620 batch remains preserved and counted. The previous inspection of !12662 in the !12663–!12712 ledger was explicitly a frontier check and was not counted as a review. The 50 highest-numbered remaining MRs happened to be the contiguous set !12662 through !12613; no numeric interval was assumed reviewed merely from neighboring ledger entries.

## Exact reviewed set

| MR | State | Review disposition |
|---|---|---|
| !12662 | merged | Deep/corroboration — Guy Harris-authored macOS build fix applies deployment/SDK flags to libpcre and zstd so dependencies respect Wireshark's minimum supported macOS target. Reinforces existing platform-build compatibility guidance. |
| !12661 | merged | Scan — fixes the VMware HeartBeat display-filter field name from an ESXi-specific spelling to the actual IPv4-address meaning. |
| !12660 | merged | Scan/corroboration — Guy Harris-authored comment correction documents that liblzma needs deployment flags because Wireshark's libxml2 dependency uses it. |
| !12659 | merged | Scan — release-4.2 backport adding IEEE 802.11 ETAG FILS Request Parameter dissection. |
| !12658 | merged | Scan — release-4.2 VMware HeartBeat dissector enhancement. |
| !12657 | merged | Review-focused — Nettrace NAS-EPS/NAS-5GS handling; review required the underlying bug to be fixed separately before this dependent change merged. Reinforces sequencing fixes at the correct layer. |
| !12656 | merged | Deep/corroboration — release-3.6 Netscreen bounds-checking security fix. Capacity is checked before each packet-buffer write rather than after it. |
| !12655 | merged | Deep/corroboration — release-4.0 backport of the Netscreen pre-write bounds check. |
| !12654 | merged | Deep/corroboration — release-4.2 backport of the Netscreen pre-write bounds check. |
| !12653 | merged | Deep/corroboration — Guy Harris-authored master Netscreen security fix; post-write bounds checks are too late. Existing buffer-capacity guidance already captures the durable rule. |
| !12652 | merged | Scan — master VMware HeartBeat dissector enhancement. |
| !12651 | merged | Scan — Windows vcpkg export update. |
| !12650 | merged | Scan — Windows nghttp3 dependency upgrade. |
| !12649 | merged | Corroboration — Sysdig uses libsinsp native-syscall support and shared Wiretap pcapng block definitions rather than duplicating block constants. |
| !12648 | merged | Scan — release-4.2 BLF interface-name leak fix. |
| !12647 | merged | Deep/promoted — BLF distinguishes `WTAP_ENCAP_NONE` from unknown/bogus encapsulation while it synthesizes IDBs, and ensures each record's link type matches its interface link type. Added `wiretap-record-interface-conventions.md`. |
| !12646 | merged | Review-focused — adds `WS_NOT_IMPLEMENTED()` as a grep-friendly developer placeholder; Guy Harris clarified that it is intended for unfinished code during development, not as a substitute for completing submitted implementation. |
| !12645 | merged | Scan — release-4.2 androiddump capture-filter support. |
| !12644 | merged | Scan — release-4.2 nghttp2 dependency upgrade. |
| !12643 | merged | Deep/promoted — display-filter constant-expression typing now looks to the semantic type of the opposite operand, removing operand-order asymmetry such as rejecting `1 == frame.number`. Added `dfilter-type-inference-conventions.md`. |
| !12642 | merged | Scan — master IEEE 802.11 ETAG FILS Request Parameter dissection. |
| !12641 | merged | Scan — master Windows nghttp2 dependency upgrade. |
| !12640 | merged | Corroboration — USB CDC follow-stream support preserves endpoint direction as part of endpoint identity and publishes USB-layer addressing before downstream dissection consumes it. |
| !12639 | merged | Scan — master BLF interface-name leak fix. |
| !12638 | merged | Scan — MAC-NR UL LCID length handling. |
| !12637 | merged | Scan — makes a TLS utility `value_string` static. |
| !12636 | merged | Scan — MAC-NR RedCap CCCH length/RRC-dispatch handling. |
| !12635 | merged | Deep/promoted — BLF CAN parsing bug fix. Lars Völker explicitly advised keeping proposed BRS/ESI enhancement work separate because mixing enhancements with bug fixes makes stable backports harder. Added `submission-backport-scope-conventions.md`. |
| !12634 | merged | Scan — restores a flexible plugin folder-type hierarchy. |
| !12633 | closed, unmerged | Lower weight — proposed NAS-EPS handling crossed into NAS-5GS semantics at the wrong abstraction layer; Pascal Quantin challenged that design, and the author redirected the actual problem to Nettrace work in !12657 before closing this MR. |
| !12632 | merged | Review-focused — BLF LIN parsing fix with dependency ordering and detailed discussion reconciling implementation with documented parity-bit behavior. |
| !12631 | merged | Scan — release-4.2 JA4 TLS client fingerprinting backport. |
| !12630 | merged | Deep/corroboration — release-4.2 HTTP/3 fix invalidates a global alias to file-scoped memory when the scope/decoder is destroyed. Reinforces existing allocator-scope stale-alias guidance. |
| !12629 | merged | Deep/corroboration — John Thacker-authored master HTTP/3 lifetime fix; a broader-lived pointer must not survive teardown of the file-scoped allocation it references, and `wmem_map` keys are borrowed rather than copied. Existing allocator-scope/container-ownership guidance already covers the rule. |
| !12628 | closed, unmerged | Lower weight/workflow evidence — DHCP option-120 offset proposal. Alexis La Goutte required a dedicated topic branch instead of the contributor's `master` and a component-style subject; Jaap Keuter pointed to the submission guide. Existing submission guidance already covers this. |
| !12627 | merged | Scan — release-4.2 formatting update keeps `proto_register_protocol` short names on the same or following line so release tooling/grep remains useful. |
| !12626 | merged | Scan/caution — release-4.2 HTTP/3/nghttp3 packaging backport. A later unresolved comment noted that Debian packaging had made nghttp3 mandatory while setup scripts still treated it as optional; useful caution but not promoted as settled convention. |
| !12625 | merged | Scan — release-4.2 reports nghttp3 runtime version when the API supports it. |
| !12624 | merged | Scan — master reports nghttp3 runtime version in addition to compiled version. |
| !12623 | merged | Review-focused — MAC-NR LCID constants/value strings; Martin Mathieson requested the customary reserved-bits field and concise value labels. Protocol-local style guidance only. |
| !12622 | merged | Review-focused — BLF object-type support with extensive format/parser review and selective logging of unsupported types; no additional general rule beyond existing Wiretap validation/logging guidance. |
| !12621 | merged | Scan — Qt capture-comment editor split into a dedicated dialog supporting multiple comments/sections and fixing small leaks. |
| !12620 | merged | Scan — master formatting update makes `grep proto_register_protocol` useful for release-note tooling. |
| !12619 | merged | Scan — release-4.2 simplification of AsciiDoc man-page markup. |
| !12618 | merged | Scan — master simplification of AsciiDoc man-page markup. |
| !12617 | merged | Scan — automatic master data/translations/numbers update. |
| !12616 | merged | Scan — automatic release-4.2 data/translations/numbers update. |
| !12615 | merged | Scan — automatic release-4.0 data/translations/numbers update. |
| !12614 | merged | Scan — automatic release-3.6 data/translations/numbers update. |
| !12613 | merged | Review-focused — HTTP/3/nghttp3 compatibility and packaging update plus CI Homebrew refresh behavior; discussion diagnosed stale runner formula metadata and made environment-version information visible before enabling normal Homebrew updates. No new rule promoted beyond existing CI/platform reproducibility guidance. |

## Durable notebook changes from this run

- `dfilter-type-inference-conventions.md` — comparison typing should use semantic context from either operand rather than making a typed-left-operand assumption. Primary evidence: merged !12643.
- `wiretap-record-interface-conventions.md` — records emitted through synthesized IDBs must have encapsulation matching the owning interface, and “not established yet” must not be represented as “unknown/bogus.” Primary evidence: merged !12647 by John Thacker.
- `submission-backport-scope-conventions.md` — keep bug fixes independently cherry-pickable from enhancements when stable backports are plausible. Primary evidence: merged !12635 and direct review from Lars Völker.

Strong accepted evidence that was intentionally treated as corroboration rather than duplicated into new rules includes Guy Harris's !12653–!12656 pre-write capacity checks, Guy's !12662 deployment-target fix, and John Thacker's !12629/!12630 scoped-memory alias invalidation.

## Frontier

MR !12612 (`Qt: Move section comments to the section area`) exists in the same corpus commit and is merged. It was inspected only to establish that the corpus continues below this batch; it is **not** included in the reviewed set above and should remain eligible for the next run unless a newer unreviewed MR appears in a subsequently updated corpus.
