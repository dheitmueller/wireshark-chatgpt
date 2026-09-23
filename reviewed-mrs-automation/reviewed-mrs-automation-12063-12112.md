# Wireshark MR review automation ledger — !12063–!12112

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Before selecting this batch, the available review tracking on notebook `main` was reconciled, including `reviewed-mrs.md`, the per-run ledgers under `reviewed-mrs-automation/`, and the immediately preceding exact ledger for !12113–!12162. The historical !17571–!17620 batch remains explicitly preserved and counted. Numeric range coverage was not assumed: the prior mention of !12112 was treated only as a frontier probe, not as a completed review.

Exact reviewed set (50 MRs): !12112, !12111, !12110, !12109, !12108, !12107, !12106, !12105, !12104, !12103, !12102, !12101, !12100, !12099, !12098, !12097, !12096, !12095, !12094, !12093, !12092, !12091, !12090, !12089, !12088, !12087, !12086, !12085, !12084, !12083, !12082, !12081, !12080, !12079, !12078, !12077, !12076, !12075, !12074, !12073, !12072, !12071, !12070, !12069, !12068, !12067, !12066, !12065, !12064, !12063

Outcome: 48 merged, 2 closed/unmerged (!12098 and !12065). Closed work is retained as lower-weight discussion evidence and is not treated as accepted implementation precedent.

| MR | Outcome | Review depth | Durable evidence / result |
|---|---|---|---|
| !12112 | Merged | Scanned | João Valverde generalizes the `ws_return*` macros to accept arbitrary expressions and moves them with assertion facilities; useful internal API cleanup, no separate convention promoted. |
| !12111 | Merged | Deep / corroboration | `addr_resolv` manufacturer lookup now carries the buffer size and rejects inputs shorter than six bytes before fixed-offset access; reinforces boundary checks at helper/API entry. |
| !12110 | Merged | Deep / corroboration | John Thacker removes an `if (tree)` guard that suppressed TACACS expert information when the Expert Info dialog dissected with a NULL tree; reinforces that semantic diagnostics must not depend on tree presentation. |
| !12109 | Merged | Backport evidence | Release-3.6 backport of !12106 nested desegmentation-state preservation. |
| !12108 | Merged | Backport evidence | Release-4.0 backport of !12106 nested desegmentation-state preservation. |
| !12107 | Merged | Scanned | Martin Mathieson recognizes E2AP/KPM timestamps as RFC 5905 section 6 and uses the shared NTP timestamp formatter. |
| !12106 | Merged | Deep / promoted | John Thacker saves and restores inherited `pinfo->can_desegment` while RTP temporarily enables its own payload desegmentation. Nested dissectors must restore caller-owned shared packet state rather than reset it to a guessed default. Promoted to `tcp-desegmentation-conventions.md`. |
| !12105 | Merged | Scanned | Gerald Combs converts Windows utility code to C99 fixed-width/native standard types; broad modernization, no new rule beyond existing type-portability guidance. |
| !12104 | Merged | Scanned | BGP Notification error-code 7 dissection updated to RFC 7313; protocol-spec correctness change. |
| !12103 | Merged | Scanned | Gerald Combs converts wsutil cryptography/checksum code to C99 types; modernization corroboration. |
| !12102 | Merged | Tooling evidence | Martin Mathieson adds a value-string capitalization checker while explicitly noting numerous legitimate exceptions; reinforces keeping source checkers narrow enough to tolerate semantic exceptions. |
| !12101 | Merged | CI corroboration | Merge-request CI explicitly enables debug code/assertions while running behavioral tests; corroborates existing CI-test-configuration guidance. |
| !12100 | Merged | Documentation corroboration | WSDG build-type documentation updated to match the revised CMake debug/release semantics. |
| !12099 | Merged | Scanned | EditorConfig/indentation correction for `exceptions.h`; style maintenance only. |
| !12098 | Closed / unmerged | Discussion-focused / down-weighted | Aruba ClearPass RADIUS dictionary update remained unmerged and was later closed; not accepted implementation precedent. |
| !12097 | Merged | Deep / architecture evidence | Martin Mathieson restructures E2AP so multiple RAN-function major versions can coexist as separate registered dissectors selected per conversation from negotiated OID/version. The MR also records unresolved tradeoffs where OIDs are reused and common semantic fields acquire version-specific filter namespaces; retained as useful architecture evidence rather than a broad rule. |
| !12096 | Merged | Scanned | Release-note maintenance only. |
| !12095 | Merged | Deep / corroboration | Master protobuf fix chooses packed repeated fixed32/fixed64 decoding width from the decoded element size rather than outer wire type 2. This is the master provenance for the behavior already seen in the previously reviewed stable backport !12137. |
| !12094 | Merged | Build-policy corroboration | RelWithDebInfo is treated as a release build without `WS_DEBUG` unless `ENABLE_DEBUG` is explicitly requested; pairs with !12101 and existing build/CI conventions. |
| !12093 | Merged | Scanned | macOS global-name-resolution tests are skipped when the application bundle cannot safely be modified by the test; platform test-environment accommodation. |
| !12092 | Merged | Deep / high-authority corroboration | Guy Harris bounds pcapng timestamp-resolution exponents before forming 64-bit scale factors, rejecting base-10 exponents >19 and base-2 exponents >63 with a concrete Wiretap error. Strongly corroborates existing arithmetic-safety guidance for externally declared timestamp scales. |
| !12091 | Merged | Scanned | Adds an IEEE 802.1CB R-TAG dissector and continues into the contained protocol; new protocol support, no distinct cross-cutting rule extracted. |
| !12090 | Merged | Scanned | Stable-branch dSYM dmgbuild configuration fix after dependency behavior changed; packaging maintenance. |
| !12089 | Merged | Scanned | dSYM dmgbuild configuration fix; packaging maintenance. |
| !12088 | Merged | Deep / corroboration | GNW replaces an assertion on an oversized packet-derived IntX value with expert information; reinforces the rule that malformed input is diagnosed rather than asserted as an internal invariant. |
| !12087 | Merged | Scanned | Adds BGP BFD strict-mode capability dissection from the then-current IETF draft. |
| !12086 | Merged | Scanned | Stable-branch macOS dmgbuild configuration fix with a conflict resolution; packaging maintenance. |
| !12085 | Merged | Scanned | Stable-branch macOS dmgbuild configuration fix; packaging maintenance. |
| !12084 | Merged | Scanned | Master macOS dmgbuild configuration fix after `biplist` ceased to be used; packaging maintenance. |
| !12083 | Merged | Scanned | Corrects TShark `--print-timers` unit presentation; localized CLI-output fix. |
| !12082 | Merged | Scanned | Adds UDS Service 0x29 certificate dissection; protocol feature work. |
| !12081 | Merged | Scanned | Corrects PIDL documentation for WKSSVC code generation. |
| !12080 | Merged | Deep / corroboration | John Thacker fixes `fvalue_t` leaks on exception-capable `proto_tree_new_item()` paths by installing a `CLEANUP_PUSH` guard and popping it only after ownership transfers to the tree. João Valverde preferred this cleaner local ownership guard over the broader !12065 allocator API. Strongly corroborates existing exception-safe allocator/lifetime conventions. |
| !12079 | Merged | Deep / promoted | Guy Harris explains that `-2147483648` is unary minus applied to a positive integer constant whose type can differ across compilers; the accepted fix uses `INT_MIN`. Promoted to `c-portability-conventions.md`. |
| !12078 | Merged | Deep / corroboration | LTP reports the payload bytes actually present when computing consumed/header lengths on truncation, preventing a negative `proto_item_set_len()`; reinforces actual-consumed-length accounting. |
| !12077 | Merged | Deep / promoted | John Thacker changes byte-offset reassembly so post-completion retransmissions set overlap metadata instead of throwing `ReassemblyError`, corrects exact-end boundary handling, documents completed-key semantics, and validates with a multipath retransmission capture. Promoted to `reassembly-conventions.md`. |
| !12076 | Merged | Deep / corroboration | SDP treats a colon with a missing attribute value as malformed but recoverable: report the error and continue parsing subsequent attributes. Reinforces bounded parser recovery instead of aborting the whole message. |
| !12075 | Merged | Portability corroboration | Fixes Visual Studio diagnostics for conversions from 64-bit `gsize`/`size_t` to 32-bit `int`; useful width-conversion review evidence. |
| !12074 | Merged | Scanned | macOS setup fixes for building several dependency libraries; platform build maintenance. |
| !12073 | Merged | Tooling corroboration | Adds `--check-value-strings` to the CI invocation of `tools/check_tfs.py`; accepted source checks should run continuously in MR pipelines. |
| !12072 | Merged | API corroboration | Uses `expert_add_info_format()` for an existing `proto_item` instead of the tree-oriented expert-add API; reinforces matching protocol-tree/expert APIs to the object actually owned. |
| !12071 | Merged | Wiretap corroboration | BLF timestamp precision becomes per-record so 10-microsecond and nanosecond records retain their actual precision metadata. |
| !12070 | Merged | Testing/lifetime corroboration | Fixes assorted leaks exposed by running the test target under LeakSanitizer; supports sanitizer-backed lifetime validation. |
| !12069 | Merged | Wiretap corroboration | Extends timestamp-precision enum support and assigns NetMon 2.3 its actual 100-ns precision while retaining compatibility aliases for older enum names. |
| !12068 | Merged | UI/configuration corroboration | Extends View/recent-file timestamp precision support across seconds through nanoseconds and normalizes enum naming. |
| !12067 | Merged | Scanned | ORAN FH CUS adds a dedicated BFW bundle subtree field and ID field; localized field-model improvement. |
| !12066 | Merged | Deep / corroboration | Fixes OUI lookup paths outside ordinary MAC name resolution and exposes an existing byte-order assumption; reinforces that shared lookup helpers need explicit representation/byte-order contracts. |
| !12065 | Closed / unmerged | Deep / superseded | Alternative fvalue-leak fix proposed wmem-backed `fvalue_t` allocation plus cleanup callbacks. João warned that fvalue cleanup remains mandatory when the scope ends. The broader approach was abandoned in favor of the merged local cleanup guard in !12080, so it is retained only as lower-weight design discussion. |
| !12064 | Merged | Backport corroboration | Stable-branch SCCP fix avoids manually shifting a value and then passing it through a masked registered field, which would shift twice; uses normal `proto_tree_add_item()` decoding. |
| !12063 | Merged | Backport corroboration | Companion stable-branch SCCP double-shift/mask fix; corroborates the existing registered-field decoding/mask semantics. |

## Notebook updates promoted in this run

- `tcp-desegmentation-conventions.md`: !12106, reinforced by !12108/!12109, establishes scoped save/restore of inherited `pinfo->can_desegment` when an inner dissector temporarily enables its own desegmentation.
- `reassembly-conventions.md`: !12077 establishes that ordinary retransmissions after completed reassembly should use overlap/provenance state instead of automatically becoming malformed exceptions, and documents how completed-entry/key-reuse semantics differ among reassembly APIs.
- `c-portability-conventions.md`: Guy Harris's !12079 establishes using the standard minimum-value macro instead of spelling a minimum signed value as unary minus applied to an out-of-range positive decimal literal.

Strong corroboration intentionally not duplicated into additional notebook rules includes Guy Harris's pcapng timestamp-scale bounds in !12092, exception-safe ownership transfer in !12080, NULL-tree-independent expert reporting in !12110, malformed-packet assertion removal in !12088, protobuf packed-element width semantics in !12095, and source-checker/CI integration in !12102/!12073.

Frontier check only (not reviewed): !12062, `tshark: let ws_strtou32() do the full check for a valid number.`, exists in the corpus and is merged. It is the next descending candidate absent newly scraped higher-numbered unreviewed material.
