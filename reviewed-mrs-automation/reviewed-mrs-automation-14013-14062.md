# Reviewed MRs automation run: !14013–!14062

## Corpus snapshot

- MR corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `62ebb08f4bf8ee016ea8fd47631fa9fdc8da297e`

## Selection method

The already-reviewed MR set was reconstructed from the available review tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run files under `reviewed-mrs-automation/`. The historical !17571–!17620 run was explicitly preserved and counted. The preceding exact ledger recorded !14063–!14112 and explicitly stated that !14062 was fetched only as a frontier probe, not reviewed. The notebook and corpus had not advanced since that reconciliation at selection time, and the corpus commit was rechecked before recording this run. Exact MR-number subtraction therefore selected the fifty highest-numbered corpus MRs absent from the reviewed set; no numeric range was assumed reviewed merely from neighboring ledger entries.

## Exact MRs reviewed

!14062, !14061, !14060, !14059, !14058, !14057, !14056, !14055, !14054, !14053, !14052, !14051, !14050, !14049, !14048, !14047, !14046, !14045, !14044, !14043, !14042, !14041, !14040, !14039, !14038, !14037, !14036, !14035, !14034, !14033, !14032, !14031, !14030, !14029, !14028, !14027, !14026, !14025, !14024, !14023, !14022, !14021, !14020, !14019, !14018, !14017, !14016, !14015, !14014, !14013.

Exactly 50 MRs were reviewed. **47 were merged.** Three were closed/unmerged and were down-weighted: **!14055**, an empty conflicting draft cherry-pick; **!14023**, whose proposed previous-displayed-timestamp change was reported by John Thacker to cause a segfault and was closed; and **!14013**, an unresolved SMB2 feature submission that was closed rather than merged.

## Per-MR review notes

| MR | Classification | Review result |
|---:|---|---|
| !14062 | Scanned | Merged NR RRC 17.7.0 ASN.1 update. Authoritative generated/specification maintenance; no new convention beyond existing generated-source rules. |
| !14061 | Scanned | Merged LTE RRC 17.7.0 ASN.1 update; same generated-source pattern. |
| !14060 | Scanned | Merged LPP 17.7.0 ASN.1 update; same generated-source pattern. |
| !14059 | **Deep / promoted** | John Thacker-authored/merged RDPUDP leak fix. File-scoped wmem trees retained cloned TVBs, but freeing the trees did not call `tvb_free()` on payloads. Promoted: scoped-container lifetime does not replace payload-specific destruction. |
| !14058 | Deep / corroboration | Guy Harris-authored/merged release-4.2 backport of the X.75/LAPB registration correction in !14057. |
| !14057 | **Deep / promoted / very high authority** | Guy Harris-authored/merged master fix removes X.75's registration for `WTAP_ENCAP_LAPB`. A fixed table key is a semantic ownership claim; similar protocols must not compete for the same single-value selector. |
| !14056 | Corroboration | Merged release-4.0 extcap/libssh SHA-1 compatibility backport. Reinforces controlled compatibility handling for legacy devices. |
| !14055 | **Down-weighted** | Closed empty draft cherry-pick with conflicts and no changes/commits. No architectural evidence. |
| !14054 | Corroboration | Merged release-4.2 backport of the extcap/libssh SHA-1 compatibility change. |
| !14053 | Deep / corroboration | Merged HTTP/3 Huffman decoding fix. Reviewer-provided capture exposed a NULL path and drove additional checks; reinforces concrete capture-based regression testing. |
| !14052 | Scanned | Merged GTPv2 Monitoring Event Information offset/length correction. Parser-coordinate bug; existing offset/accounting guidance covers it. |
| !14051 | Scanned | Merged Falco PID/CPU lookup cache improvement. Hot-path performance work without a new general convention. |
| !14050 | Corroboration | Merged removal of duplicate `interface_t` members in favor of embedded `if_info_t` state; reinforces single-source-of-truth state design. |
| !14049 | Corroboration | Merged release-4.2 NBSS short-packet backport of !14030. |
| !14048 | Corroboration | Merged stable GeoIP offset-precedence backport. |
| !14047 | Corroboration | Merged stable GeoIP offset-precedence backport. |
| !14046 | Corroboration | Merged stable GeoIP offset-precedence backport. |
| !14045 | Scanned | Merged master GeoIP offset-precedence correction; straightforward C expression correctness. |
| !14044 | Scanned | Merged automatic protocol/data update; maintenance only. |
| !14043 | Scanned | Merged automatic protocol/data update; maintenance only. |
| !14042 | Scanned | Merged automatic protocol/data update; maintenance only. |
| !14041 | Scanned | Merged automatic protocol/data update; maintenance only. |
| !14040 | Deep / corroboration | Merged master extcap/libssh compatibility change explicitly opts into legacy SHA-1 for older hardware. Discussion recognizes that dependency/security defaults and deployment baselines vary by platform; existing compatibility/dependency policy already covers the broader lesson. |
| !14039 | Scanned | Merged Falco conversation-filter support; feature-specific. |
| !14038 | Scanned | Merged Logray Syscalls profile update; profile/data maintenance. |
| !14037 | Scanned | Merged SOME/IP-SD `val_to_str_const()` cleanup; helper/API cleanup without a new rule. |
| !14036 | Scanned | Merged Falco memory-statistics diagnostics; observability feature. |
| !14035 | Corroboration | Merged RTP stable backport preserving extended sequence/timestamp number-space state across repeated setup messages. |
| !14034 | Discussion-focused / corroboration | Merged Zigbee ZDP parsing fix. Anders Broman questioned expressing a one-byte wire advance as `sizeof(guint8)`; reinforces that wire offsets should communicate protocol widths rather than implementation-type storage size. Existing wire-layout guidance is sufficient. |
| !14033 | Deep / corroboration | Merged master RTP fix shares per-SSRC extended sequence/timestamp wrap state when repeated setup creates a new conversation for the same stream; reinforces continuity of logical protocol state across setup churn. |
| !14032 | Scanned | Merged RTMPT default-chunk-size preference for captures beginning mid-session where negotiated state was missed. Useful recovery feature; no distinct rule promoted. |
| !14031 | Scanned | Merged Qt status-bar update cleanup; UI implementation maintenance. |
| !14030 | Corroboration | Merged master NBSS fix requests desegmentation for header-short packets that can plausibly be the start of a message; reinforces existing short-packet/desegmentation guidance. |
| !14029 | Corroboration | Merged release-3.6 backport of the Kerberos pointer-cast cleanup in !14015. |
| !14028 | Corroboration | Merged release-4.0 backport of !14015. |
| !14027 | Corroboration | Merged release-4.2 backport of !14015. |
| !14026 | Deep / corroboration | Merged SANE dissector. Separates PDU-length probing from full dissection so stream desegmentation can ask for more bytes before the main parser assumes completeness; reinforces existing TCP/PDU parsing practice. |
| !14025 | Scanned | Merged Falco bridge change replaces `sscanf` with Wireshark integer parsing after checking the required prefix, improving a hot path. |
| !14024 | Deep / corroboration | Merged pcapng Sysdig fix only sets `WTAP_HAS_TS` when the event actually has a timestamp. Reinforces the existing capture-metadata presence rule: presence flags describe data genuinely present, not fields merely available in the record structure. |
| !14023 | **Down-weighted / negative evidence** | Closed previous-displayed-timestamp restructuring. John Thacker reported that it caused a segfault when setting a time reference; rejected direction, not accepted architecture. |
| !14022 | Scanned | Merged release-note entry documenting removal of the Parlay dissector. |
| !14021 | Scanned | Merged CI documentation-job dependency fix (`ruby-dev`). Build maintenance only. |
| !14020 | Corroboration | Merged release-3.6 Parlay recursion-guard backport; reinforces bounded recursion. |
| !14019 | Corroboration | Merged release-4.0 Parlay recursion-guard backport. |
| !14018 | Corroboration | Merged release-4.2 Parlay recursion-guard backport. |
| !14017 | Scanned | Merged retirement of the generated Parlay dissector while temporarily retaining its IDL inputs for reversibility/regeneration. Consistent with source-of-truth preservation, no separate rule needed. |
| !14016 | Corroboration | Merged OPC UA certificate parsing fix manually patches generated output because the generator was not working. Exceptional generated-file maintenance; existing generated-source/source-of-truth guidance already covers the need to understand and document that boundary. |
| !14015 | Discussion-focused / corroboration | Merged Kerberos portability fix. Pascal Quantin steered the patch toward the simpler direct pointer cast rather than adding an integer-cast dependency merely as documentation; accepted stable backports are !14027–!14029. |
| !14014 | **Deep / promoted** | John Thacker-authored/merged RTMPT fix records every frame contributing to custom unchunking and marks those frames as dependencies so selected-packet export retains required inputs. Promoted: custom reassembly must reproduce framework dependency bookkeeping, not only reconstruct bytes. |
| !14013 | **Down-weighted** | Closed/unmerged SMB2 FSCTL/QueryInfo/server-notification work with unresolved review state. Accepted later work is stronger evidence; no rule extracted. |

## Durable findings promoted to the notebook

### Fixed dissector-table registrations are semantic ownership claims

Merged master !14057, authored and merged by Guy Harris, removes X.75's `WTAP_ENCAP_LAPB` registration because LAPB captures belong to the LAPB dissector. Similar framing does not justify having unrelated dissectors compete for one fixed selector. Release-4.2 !14058 corroborates the correction. Added to `dissector-entry-and-state-conventions.md`.

### Custom reassembly must preserve dependency metadata used outside dissection

Merged master !14014, authored and merged by John Thacker, makes RTMPT's manual unchunking record all contributing frames and call `mark_frame_as_depended_upon()` when reconstruction completes. Without those dependencies, exporting selected packets can omit earlier frames required for the selected reconstructed PDU. Added to `reassembly-conventions.md`, including an export-oriented testing rule.

### Scoped containers do not automatically destroy resource-owning payloads

Merged master !14059, authored and merged by John Thacker, fixes cloned TVBs retained in file-scope wmem trees. File-scope teardown freed the tree storage but not the cloned TVBs; a file-scope callback now walks the trees and calls `tvb_free()` on each payload. Added to `allocator-scope-conventions.md`.

## Strong corroborating findings not duplicated as rules

- !14053: a reviewer-provided HTTP/3 capture exposed a NULL path after Huffman decoding work, reinforcing sample-driven regression testing.
- !14040 plus !14054/!14056: libssh security defaults can change before all deployed endpoints do; legacy interoperability should be explicit and scoped, while platform dependency baselines may differ.
- !14033/!14035: repeated setup of the same RTP stream must not reset extended sequence/timestamp number-space state.
- !14030/!14049: a too-short first NBSS segment should request desegmentation when it plausibly begins a valid message instead of being prematurely classified as continuation data.
- !14024: capture-record presence flags must reflect whether metadata such as a timestamp actually exists.
- !14020/!14019/!14018: bounded protocol recursion remains release-worthy safety behavior.
- !14016/!14017: generated outputs and authoritative generator inputs have distinct lifecycle/source-of-truth roles; exceptions such as a temporarily broken generator should be explicit rather than obscuring that distinction.
- !14023 and !14013 were not treated as preferred architecture because they were closed/unmerged; !14055 contained no substantive change at all.

## Notebook commits from this run

- `d496bf13d54837fa35f44b31af46c11222d5073c` — fixed dissector-table registration ownership rule.
- `730502c4dbe4c5bf288837f0936224bd2f2dab27` — custom reassembly dependency-bookkeeping rule.
- `7f8354dca9958010096f9779fa17dc0d37604850` — scoped-container payload cleanup rule.

## Frontier

`mr_14012.json` exists at the corpus commit above and is merged. It was fetched only to verify that the corpus continues and was **not** counted or ledgered as reviewed. Unless a later corpus snapshot introduces a higher-numbered unreviewed MR, !14012 is the next descending candidate.
