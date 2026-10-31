# Automated Wireshark MR review — !8711 through !8661, excluding absent !8696

**Review date:** 2026-10-31. **Model:** GPT-6 (newer than GPT-5.6 Sol).
**Corpus repository:** `dheitmueller/wireshark-corpus-mrs`
**Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
**Direction:** highest-numbered previously unreviewed MR toward older.
**Exact count:** 50 distinct corpus MRs: 49 merged and 1 closed/unmerged (!8706).
**Missing corpus record:** `mr_8696.json` does not exist at the pinned corpus commit; !8696 was not reviewed and is **not** counted. Thus this batch is intentionally noncontiguous.

## Selection and overlap checks

Before selecting these entries, the notebook Git tree was inventoried (372 per-run and supplemental files under `reviewed-mrs-automation/`), `reviewed-mrs.md`, the supplemental aggregate ledger, the immediately preceding exact 50-MR ledger (!8712–!8761), and all 16 exceptional / mixed / skip / gap / backfill / exact-membership tracking files were checked. Previous ledger mentions !8711 only as a **next candidate**, not as reviewed. No completed prior-review membership was identified in this selected set; no numeric interval was presumed reviewed solely because a ledger mentioned a few values in it.

Historical !17571–!17620 ledger is preserved: **50 distinct table entries** with Git blob `9393377c3adeb3a38d502532839765e3abe771fe`. No preexisting tracking was edited.

## Durable observations and authority weighting

- **Incorrect merged code is not a rule:** merged !8711 passed the pretty-printed IPP label as an FT_STRING value. **João Valverde explicitly warned against backporting it**; later accepted !8731/!8724 rollback-and-repair separates decoded field value from display text. The historical merge alone should not override the review and corrective chain.
- **Unicode has separate contracts for literal values, protocol decoded values, and UI presentation:** merged !8677 (João) stops mutating generic ftype strings and pushes diagnostic checks to `proto_tree_add_string`; !8688 (João) uses printable DNS names in columns; !8697/!8695 (John Thacker) maintain valid codepoint boundaries and NUL termination; !8665 (João) uses `proto_tree_add_item_ret_display_string`; !8674/!8664 prevent copying wire-byte lengths into Unicode output limits.
- **Prefer eliminating fragile buffers to enlarging them:** João's merged !8703 and stable backport !8705 enlarged a timezone buffer, but **Guy Harris's merged !8708** removes it entirely and passes timezone name and separator to a scoped allocator-backed formatter.
- **Profile bootstrap semantics:** merged !8686 (John Thacker) repairs global-to-personal profile copy before the profile filename index exists. **Stig Bjørlykke** strongly defended filtering profile-owned files in normal operation; exceptional early copying requires a justified global-profile scope and a shared filesystem helper. Later !9133 should be considered when inspecting current implementation.
- **IPv6 taxonomy and review usability:** merged !8666 was materially narrowed under **João Valverde's** review. Distinguish address space from special-purpose registry classifications, avoid user-hostile warnings on perfectly useful documentation addresses, reject obsolete site-local classifications and expose optional meaningful FT_STRING fields.
- **Generator/spec provenance:** Guy's merged !8698 corrects the E2AP maxofRICrequestID constant using newer O-RAN v2.03 evidence after older v2.01 supplied an out-of-range bound; Martin's merged !8700 documents the newer spec. Generated code and ASN.1 source must stay synchronized (!8690).
- **Static analysis is not closed by a green merge:** Alexis La Goutte's post-iteration comments on merged !8676 still list RTPS null-dereference and undefined-value diagnostics. Distinguish actual data-path safety from silencing a subset of warnings.
- **CI economics:** Gerald's closed !8706 mold-linker experiment did not improve throughput on shared runners; do not use it as policy. His merged !8669 (with !8670/!8671 backports) instead allows RPM builds to use more ninja workers through `RPM_BUILD_NCPUS`.

## Exact MR assessments

| MR | Outcome | Depth | Assessment |
| --- | --- | --- | --- |
| [!8711](https://gitlab.com/wireshark/wireshark/-/merge_requests/8711) | merged | deep | IPP string-field crash workaround passed a formatted label as the actual value; João Valverde objected post-merge and explicitly opposed backporting. Correct semantic field repair is !8724 (after rollback !8731). |
| [!8710](https://gitlab.com/wireshark/wireshark/-/merge_requests/8710) | merged | scanned | João Valverde adds a NULL guard in the UTF-8 check macro; addresses a separate #18563 failure path. |
| [!8709](https://gitlab.com/wireshark/wireshark/-/merge_requests/8709) | merged | scanned | Gerald Combs backports Qt 5/6 QFont serialization compatibility to release-4.0; master change is !8691. |
| [!8708](https://gitlab.com/wireshark/wireshark/-/merge_requests/8708) | merged | deep | Guy Harris removes fixed-size timezone-name buffer and constructs separator/name directly in scoped formatted output; supersedes !8703/!8705 capacity increase. |
| [!8707](https://gitlab.com/wireshark/wireshark/-/merge_requests/8707) | merged | scanned | NSIS component grouping and Windows installation guide revised to expose extcap components. |
| [!8706](https://gitlab.com/wireshark/wireshark/-/merge_requests/8706) | closed/unmerged | discussion | Closed draft mold linker experiment; Gerald Combs found little/no speed improvement on shared, possibly single-CPU GitLab runners. Not accepted CI policy. |
| [!8705](https://gitlab.com/wireshark/wireshark/-/merge_requests/8705) | merged | scanned | Release-4.0 backport of interim timezone buffer enlargement; later master !8708 removes buffer. |
| [!8704](https://gitlab.com/wireshark/wireshark/-/merge_requests/8704) | merged | scanned | Veritas LLT adds new packet form with two-byte length and source/destination node IDs. |
| [!8703](https://gitlab.com/wireshark/wireshark/-/merge_requests/8703) | merged | deep | João Valverde enlarges timezone display buffer to avoid truncated/incomplete multibyte locale names; subsequently replaced architecturally by !8708. |
| [!8702](https://gitlab.com/wireshark/wireshark/-/merge_requests/8702) | merged | scanned | Move type/field checking tools to end of CI test log for diagnostic visibility; Martin Mathieson asked how much they catch. |
| [!8701](https://gitlab.com/wireshark/wireshark/-/merge_requests/8701) | merged | scanned | Metamako local true_false_string gets static linkage. |
| [!8700](https://gitlab.com/wireshark/wireshark/-/merge_requests/8700) | merged | discussion | Martin Mathieson documents E2AP maxofRICrequestID value coming from later v2.03 specification after Guy's !8698 correction. |
| [!8699](https://gitlab.com/wireshark/wireshark/-/merge_requests/8699) | merged | discussion | Guy Harris fixes title-case GUI menu labels and dialog ellipses; Stig Bjørlykke questioned ASCII three dots versus Unicode ellipsis. |
| [!8698](https://gitlab.com/wireshark/wireshark/-/merge_requests/8698) | merged | deep | Guy Harris fixes E2AP ASN.1 constant overflow: revised O-RAN v2.03 bound 1024 instead of 4294967295 (out of int range); Martin Mathieson traces older-spec provenance. |
| [!8697](https://gitlab.com/wireshark/wireshark/-/merge_requests/8697) | merged | deep | John Thacker fixes column UTF-8 truncation: check snprintf length, truncate to whole code points and use ws_label_strcpy instead of unsafe byte concatenation. |
| [!8695](https://gitlab.com/wireshark/wireshark/-/merge_requests/8695) | merged | scanned | John Thacker ensures ws_label_strcpy NUL-terminates destination on empty or invalid UTF-8 when room exists. |
| [!8694](https://gitlab.com/wireshark/wireshark/-/merge_requests/8694) | merged | scanned | Bluetooth LE procedure collision logic accounts for control procedures without responses. |
| [!8693](https://gitlab.com/wireshark/wireshark/-/merge_requests/8693) | merged | scanned | Release-4.0 backport of Qt 6 Windows default font-size adjustment from !8692. |
| [!8692](https://gitlab.com/wireshark/wireshark/-/merge_requests/8692) | merged | scanned | Gerald Combs differentiates Qt 6 Windows monospace font-size handling. |
| [!8691](https://gitlab.com/wireshark/wireshark/-/merge_requests/8691) | merged | deep | Gerald Combs prevents Qt5 QFont::fromString from parsing Qt6's longer/differently encoded saved QFont::toString preferences; fallback to installed defaults. |
| [!8690](https://gitlab.com/wireshark/wireshark/-/merge_requests/8690) | merged | deep | E2AP 2.01 / service-model ASN.1 upgrade, generated/template sync and conversation-based RAN function mapping; discussion flags shortString TVB bounds. |
| [!8689](https://gitlab.com/wireshark/wireshark/-/merge_requests/8689) | merged | deep | Martin Mathieson fixes PER restricted-string tree byte-span ceil of exclusive bit end; same correction later appears in !8723/!8722. |
| [!8688](https://gitlab.com/wireshark/wireshark/-/merge_requests/8688) | merged | deep | João Valverde ensures DNS Info-column values use printable UTF-8 display name, not raw unescaped DNS name bytes; retains field/column distinction. |
| [!8687](https://gitlab.com/wireshark/wireshark/-/merge_requests/8687) | merged | scanned | AMQP fixes character/type tag display encoding using bounded properly terminated representation. |
| [!8686](https://gitlab.com/wireshark/wireshark/-/merge_requests/8686) | merged | deep | John Thacker fixes global-profile copy during early CLI startup when profile_files index is absent; Stig Bjørlykke requests explicit special-case and shared filesystem helper. |
| [!8685](https://gitlab.com/wireshark/wireshark/-/merge_requests/8685) | merged | scanned | PacketCable/VTP timestamp display avoids raw-encoding assumptions by parsing numeric components before formatting. |
| [!8684](https://gitlab.com/wireshark/wireshark/-/merge_requests/8684) | merged | scanned | EVS D-bits enum 0x6 corrected to NB 24.4 kbps per 3GPP, with backport requested. |
| [!8683](https://gitlab.com/wireshark/wireshark/-/merge_requests/8683) | merged | scanned | IEEE 802.15.4 6top sub-IE final assignment added while preserving draft identifier. |
| [!8682](https://gitlab.com/wireshark/wireshark/-/merge_requests/8682) | merged | scanned | Routine release-3.6 version bump from 3.6.9 to 3.6.10. |
| [!8681](https://gitlab.com/wireshark/wireshark/-/merge_requests/8681) | merged | scanned | Routine release-4.0 version bump from 4.0.1 to 4.0.2. |
| [!8680](https://gitlab.com/wireshark/wireshark/-/merge_requests/8680) | merged | scanned | Release-3.6.9 packaging/build-note preparation. |
| [!8679](https://gitlab.com/wireshark/wireshark/-/merge_requests/8679) | merged | scanned | Release-4.0.1 packaging/build-note preparation. |
| [!8678](https://gitlab.com/wireshark/wireshark/-/merge_requests/8678) | merged | scanned | NSIS uninstaller stops assuming extcap binaries live alongside main program executable. |
| [!8677](https://gitlab.com/wireshark/wireshark/-/merge_requests/8677) | merged | deep | João Valverde moves UTF-8 checks to proto_tree string insertion rather than silently sanitizing generic ftype strings; display-filter literals retain exact bytes. |
| [!8676](https://gitlab.com/wireshark/wireshark/-/merge_requests/8676) | merged | deep | RTPS Clang warnings refactor; Alexis La Goutte identifies remaining null-dereference and uninitialized-value analyzer paths. Merged does not prove complete analyzer cleanup. |
| [!8675](https://gitlab.com/wireshark/wireshark/-/merge_requests/8675) | merged | scanned | Netrom and legacy FiveCo remove arbitrary length clipping after decoding text. |
| [!8674](https://gitlab.com/wireshark/wireshark/-/merge_requests/8674) | merged | deep | João Valverde audits many dissectors for truncating UTF-8 decoded strings to original wire byte lengths, which can split code points. |
| [!8673](https://gitlab.com/wireshark/wireshark/-/merge_requests/8673) | merged | scanned | Release-4.0 backport to fix Enabled Protocols proxy search when filtering enabled/disabled rows. |
| [!8672](https://gitlab.com/wireshark/wireshark/-/merge_requests/8672) | merged | scanned | Synergy uses UTF-8-safe presentation for unknown packet type and fixes handshake byte selection. |
| [!8671](https://gitlab.com/wireshark/wireshark/-/merge_requests/8671) | merged | scanned | Release-3.6 backport of RPM parallel build configuration from !8669. |
| [!8670](https://gitlab.com/wireshark/wireshark/-/merge_requests/8670) | merged | scanned | Release-4.0 backport of RPM parallel build configuration from !8669. |
| [!8669](https://gitlab.com/wireshark/wireshark/-/merge_requests/8669) | merged | discussion | Gerald Combs sets RPM_BUILD_NCPUS so ninja makes use of CPUs in Fedora/Rocky RPM builds; backported. |
| [!8668](https://gitlab.com/wireshark/wireshark/-/merge_requests/8668) | merged | scanned | Routine 3.6.9 release-note preparation. |
| [!8667](https://gitlab.com/wireshark/wireshark/-/merge_requests/8667) | merged | scanned | Routine 4.0.1 release-note preparation. |
| [!8666](https://gitlab.com/wireshark/wireshark/-/merge_requests/8666) | merged | deep | IPv6 address-space fields added behind opt-in preference; João Valverde narrowed taxonomy, rejected deprecated site-local use and noisy expert warnings, and required FT_STRING/consistent filter names. |
| [!8665](https://gitlab.com/wireshark/wireshark/-/merge_requests/8665) | merged | deep | MPEG descriptors use proto_tree_add_item_ret_display_string for ASCII ISO-639 tags; presentation comes from validated decoded string, not raw stack octets. |
| [!8664](https://gitlab.com/wireshark/wireshark/-/merge_requests/8664) | merged | scanned | DNS HINFO CPU/OS strings avoid truncation by original encoded byte length after conversion to UTF-8. |
| [!8663](https://gitlab.com/wireshark/wireshark/-/merge_requests/8663) | merged | scanned | Release-3.6 backport of !8661 IEEE 802.11 status-label length fix. |
| [!8662](https://gitlab.com/wireshark/wireshark/-/merge_requests/8662) | merged | scanned | Release-4.0 backport of !8661 IEEE 802.11 status-label length fix. |
| [!8661](https://gitlab.com/wireshark/wireshark/-/merge_requests/8661) | merged | deep | Gerald Combs checks actual wmem_strbuf_get_len instead of unused stale sts_len when formatting IEEE 802.11 DSCP status list. |

**Depth meaning:** `deep` means relevant implementation and substantive discussion inspected; `discussion` means maintainer discussion and pertinent diff examined; `scanned` means metadata, diff scope, and available discussion assessed. Captures were not replayed; no independent build, parser tests, or fuzzing were executed.

**Next candidate:** !8660 exists in the same corpus snapshot, subject to the next run's full membership check.
