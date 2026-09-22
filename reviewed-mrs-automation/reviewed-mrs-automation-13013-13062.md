# Automated MR review ledger: !13013–!13062

Reviewed 2026-09-22 using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were reviewed in this run. The already-reviewed set was reconstructed from the available per-run files under `reviewed-mrs-automation/`, the aggregate tracking there, and `reviewed-mrs.md`; the historical !17571–!17620 batch was explicitly preserved and counted. The immediately preceding ledger inspected !13062 only as a frontier probe, so it was not counted as previously reviewed. The selected numbers happen to be contiguous, but selection was based on the explicit reviewed set and corpus membership, not an assumed reviewed numeric range.

Outcome summary: 45 merged and 5 closed/unmerged (!13049, !13045, !13028, !13023, !13022). Merged changes were weighted most heavily. Closed !13049 and !13028 were used only as lower-weight review/process evidence where their discussions were unusually informative; the other closed proposals did not establish durable implementation precedent.

| MR | Outcome | Depth | Review result |
|---|---|---|---|
| !13062 | merged | Deep | WebSocket replaces a hard-coded maximum unmasked payload with a preference, keeps a conservative default, warns with expert info when only part of a payload is unmasked, and documents the performance impact of large values. Corroborates configurable resource-limit/diagnostic guidance; no duplicate rule added. |
| !13061 | merged | Deep | John Thacker fixes AVSP FCS handling by leaving Ethernet trailer/FCS classification to the outer dissector that has the complete frame context and using `set_actual_length()` to expose the semantic boundary. Strong corroboration of `framing-boundary-conventions.md`. |
| !13060 | merged | Deep | John Thacker fixes HomePNA FCS handling: when no piggybacked payload exists it sets actual length so outer Ethernet can recognize trailer/FCS; when a child protocol exists it does not pretend a nested dissector can validate a full-frame FCS. Strong framing-boundary corroboration. |
| !13059 | merged | Scanned | OpenFlow v1 FLOW_MOD adds Actions; protocol extension with no new cross-cutting convention. |
| !13058 | merged | Discussion-focused | BMP adds draft TLV support. Review discussion was predominantly local style/indentation cleanup before merge; no durable architectural rule extracted. |
| !13057 | merged | Scanned | SMB3 UNIX Extensions fixes/additions; accepted protocol-specific work, no new reusable convention identified. |
| !13056 | merged | Deep/tooling | Martin Mathieson expands copy/paste detection to look several nearby fields back and uses the results to identify additional real duplicated filter abbreviations. Promoted as bounded human-edit-pattern guidance in `ci-tooling-conventions.md`. |
| !13055 | merged | Scanned | MIKEY skips unknown payload types consistently with already out-of-range types. Corroborates graceful unknown-enum handling. |
| !13054 | merged | Deep/discussion | A copied iperf2 display-filter abbreviation was missed because the source field was four entries earlier. Martin explains checker thresholds and extends the consecutive search to a bounded five-entry lookback, directly motivating !13056. Promoted with !13056 to `ci-tooling-conventions.md`. |
| !13053 | merged | Scanned | Commit Check ensures `GITLAB_TOKEN` is set. CI integration repair; existing CI-environment guidance suffices. |
| !13052 | merged backport | Scanned/corroboration | Release-4.2 WLAN header-field registrations use integer zero rather than pointer `NULL`; corroborates the registration-handle zero-initialization migration already recorded. |
| !13051 | merged backport | Scanned/corroboration | Release-4.2 RSVP adds a real header field where historical `hf_text_only`/zero behavior was no longer valid. Corroborates registration-handle migration effects. |
| !13050 | merged | Scanned | LDAP fixes a doubled `%` format escape that belongs in generated `.cnf` input but not the resulting template C. Narrow generator/template distinction; no new rule promoted. |
| !13049 | closed, unmerged | Deep/down-weighted | Proposed strict conversation/reassembly identity including interface/VLAN evolved repeatedly and was later closed amid supersession uncertainty around !14279. Useful historical context but not accepted precedent. |
| !13048 | merged | Deep | John Thacker handles TNS Connect Data that arrives in the next Data PDU by mutating conversation state only on first pass and recording packet-local classification for stable redissection; also derives extended Data Descriptor PDU length and requests one more segment when insufficient bytes exist. Strong corroboration of request/response/state and reassembly guidance. |
| !13047 | merged | Scanned | Master RSVP missing-header-field correction corresponding to !13051. Registration migration fallout already covered. |
| !13046 | merged backport | Scanned | Release-4.2 backport of the HTTP/3 WebTransport stream-type correction from !13037. |
| !13045 | closed, unmerged | Low | Temporary proposal to make pre-commit failures non-fatal; closed almost immediately and not used as policy evidence. |
| !13044 | merged | Scanned | Corrects Vim swap-file ignore pattern from authoritative Vim naming behavior. Repository hygiene only. |
| !13043 | merged | Scanned | RF4CE removes unused locals; simple cleanup. |
| !13042 | merged | Scanned | ICMPv6 PvD ID option improvement; protocol-specific. |
| !13041 | merged | Scanned/corroboration | RTPproxy fixes an unaligned access reported by UBSan. Corroborates existing alignment-safe access guidance. |
| !13040 | merged | Scanned/corroboration | Master WLAN header-field registration correction paired with !13052. Existing registration-handle guidance covers it. |
| !13039 | merged | Scanned | SMB2 prevents `wmem_array_append()` with an invalid pointer. Defensive pointer-state correction; existing ownership/state rules suffice. |
| !13038 | merged | Scanned/corroboration | PER fixes an extension-addition-group predicate missed during the static protocol-value initialization migration. Reinforces the need to audit all validity predicates when changing sentinel representation. |
| !13037 | merged | Discussion-focused | HTTP/3 corrects WebTransport 0x54 as a stream type rather than frame type. Apparent unrelated Commit Check failures were traced to a checker issue rather than prompting unrelated source cleanup. No separate rule promoted. |
| !13036 | merged | Deep/corroboration | IEEE1905 cleanup fixes fragmented reassembly, loop overflow risk, and return length so outer padding/FCS is recognized; includes focused captures with/without padding, fragmentation, and FCS. Strong corroboration of framing and targeted-capture testing practices. |
| !13035 | merged backport | Scanned/corroboration | Release-4.2 OPA MAD unaligned-access fix corresponding to !13034. |
| !13034 | merged | Scanned/corroboration | OPA MAD fixes an unaligned access introduced by earlier work. Existing portability/alignment guidance covers it. |
| !13033 | merged backport | Scanned | Zigbee Direct renames a local `time` variable to satisfy checkAPI; narrow source-checker cleanup. |
| !13032 | merged backport | Deep/corroboration | Release-4.2 backport of !13031's HTTP strict chunk-size parsing fix. |
| !13031 | merged | Deep | John Thacker fixes HTTP chunk detection where `sscanf("%x")` accepted hexadecimal-looking prefixes of request methods such as `CCM_POST`/`BCOPY`. Accepted code strips only grammar-permitted suffix syntax and uses checked whole-token `ws_hexstrtou32()`. Promoted to `bounded-text-numeric-parsing-conventions.md`. |
| !13030 | merged backport | Scanned | Release-4.2 backport of a Qt “show bytes as image” typo fix. |
| !13029 | merged | Scanned | Master Qt “show bytes as image” typo fix; presentation-only. |
| !13028 | closed, unmerged | Deep/discussion | Proposed release-4.2 backport of the zero-initialized registration-handle migration. Stig Bjørlykke, Peter Wu, Alexis La Goutte, and Gerald Combs converged against a broad invariant change on stable due unknown side effects, release timing, and cherry-pick conflicts. Promoted as explicitly lower-weight stable-branch review-policy evidence in `registration-handle-initialization-conventions.md`. |
| !13027 | merged backport | Scanned/high-authority | Guy Harris-authored release-4.2 preference wording clarifies that PNIO expects a directory containing GSD files, not a “network path” or a single file. High-authority but narrow UI wording evidence. |
| !13026 | merged | Discussion-focused | BLF extracts channel/port names from AppText metadata XML. The author notes a proper XML parser would be preferable, but accepted review contains no broad architectural directive; no general rule inferred. |
| !13025 | merged | Scanned | Master Zigbee Direct checkAPI variable-name cleanup paired with !13033. |
| !13024 | merged | Scanned/high-authority | Guy Harris authored and merged the PNIO GSD directory-preference wording cleanup. Weighted highly but intentionally not generalized beyond clear preference semantics. |
| !13023 | closed, unmerged | Low | Draft display-filter dead-initialization experiment; not merged and not used as durable evidence. |
| !13022 | closed, unmerged | Low | German translation typo proposal, closed without merge; no durable rule. |
| !13021 | merged | Scanned/corroboration | CoAP/TLS removes explicit common-list protocol initialization as part of the zero-initialization direction. Existing registration guidance covers it. |
| !13020 | merged | Scanned | GitLab Commit Check uses the correct repository context. CI plumbing correction; no additional rule beyond existing tool-context guidance. |
| !13019 | merged | Scanned | OPC UA fixes a Coverity-reported potential null dereference. Defensive check; existing null/state rules suffice. |
| !13018 | merged | Scanned | OPC UA decryption returns the correct failure value when decryption fails, following review from earlier !12991. Error-contract correction; no new separate rule. |
| !13017 | merged | Scanned | Commit Check result text update; presentation only. |
| !13016 | merged | Scanned/corroboration | ISAKMP value strings refreshed from the IANA registry. Corroborates authoritative-registry sourcing. |
| !13015 | merged | Scanned | InfiniBand fixes spacing/formatting for unknown opcode Info-column output; localized presentation fix. |
| !13014 | merged backport | Scanned | Release-4.2 display-filter nested-function crash fix restores a removed jump that was not always a no-op. Useful regression evidence but no new cross-cutting rule beyond preserving control-flow semantics during simplification. |
| !13013 | merged | Deep | RADIUS Message-Authenticator validation received extensive Pascal Quantin review: prove the full authenticator region exists before fixed-offset access, do not validate when required packet bytes were not captured, and apply the same safety to existing authenticator code. Promoted to `validation-availability-conventions.md`; truncation is validation-unavailable, not authentication-failed. |

## Durable notebook updates

- `validation-availability-conventions.md`: cryptographic/authenticator validation requires the complete authenticated byte region; bounds-check reads/copies and classify capture truncation as validation unavailable rather than failed (!13013).
- `bounded-text-numeric-parsing-conventions.md`: when a grammar defines a numeric token, normalize only syntax explicitly permitted by the grammar and require whole-token checked conversion rather than accepting a numeric prefix (!13031, !13032).
- `registration-handle-initialization-conventions.md`: a broad representation/invariant migration accepted on master should not automatically be backported to a stable branch merely to reduce hypothetical future cherry-pick work; stable-branch risk and validation requirements differ (!13028, explicitly down-weighted as unmerged policy evidence).
- `ci-tooling-conventions.md`: source-checker heuristics for copy/paste mistakes should model the bounded short-range editing pattern that produces them, not exact adjacency only; findings remain subject to false-positive triage (!13054, !13056).

## Strong corroboration retained without duplicate rules

- !13061, !13060, and !13036 reinforce `framing-boundary-conventions.md`: the layer with full frame context owns Ethernet trailer/FCS classification, and semantic tvbuff boundaries should expose padding/trailers to that layer.
- !13048 reinforces `request-response-state-conventions.md`: mutate conversation correlation state on first pass and persist enough packet-local classification that redissection/random access does not depend on replaying state transitions.
- !13040/!13052, !13038, !13047/!13051, and !13021 reinforce the already-recorded zero-initialized registration-state migration and its predicate-audit requirement.

## Exact reviewed MR set

`!13062, !13061, !13060, !13059, !13058, !13057, !13056, !13055, !13054, !13053, !13052, !13051, !13050, !13049, !13048, !13047, !13046, !13045, !13044, !13043, !13042, !13041, !13040, !13039, !13038, !13037, !13036, !13035, !13034, !13033, !13032, !13031, !13030, !13029, !13028, !13027, !13026, !13025, !13024, !13023, !13022, !13021, !13020, !13019, !13018, !13017, !13016, !13015, !13014, !13013`
