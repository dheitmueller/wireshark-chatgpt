# Review findings: 3211-3260

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

| MR | Outcome | Review note |
|---|---|---|
| 3260 | merged | Shared wsutil binary-file writer removes duplicate UI/CLI/export code. Windows x86 exposed a resident-buffer length mismatch; Guy Harris explains why the memory-buffer domain is size_t. |
| 3259 | merged | Automatic release-3.2 registry/documentation refresh; no durable coding convention extracted. |
| 3258 | merged | Automatic release-3.4 registry/documentation refresh; no durable coding convention extracted. |
| 3257 | merged | Automatic master registry/data refresh; no durable coding convention extracted. |
| 3256 | merged | Introduces first-class opaque pcapng Custom Blocks, preserves PEN/payload, and honors copy/no-copy semantics. Later generic extension dispatch supersedes its hard-coded interpretation structure. |
| 3255 | merged | TCP out-of-order reassembly is made conditional on sequence analysis because the reassembly algorithm depends on sequence state; fixes port-reuse inconsistency. |
| 3254 | merged | Static-analysis cleanup replaces an ineffective NULL branch with DISSECTOR_ASSERT for a caller-contract invariant that is unconditionally dereferenced. |
| 3253 | merged | Qt SequenceDialog removes redundant NULL checks and documents the event-pointer precondition with an assertion; low broader impact. |
| 3252 | merged | IPsec exported helper stops passing NULL to a UAT error-output parameter. Martin Mathieson notes the tension between currently uncalled exported APIs and external callers. |
| 3251 | merged | Qt filter Cancel work favors model/lifecycle semantics over an ad-hoc reload; eventual dialog destruction/recreation makes rollback behavior explicit. |
| 3250 | merged | Qt filter dialog rejects invalid newly entered filter syntax; focused UI correctness change. |
| 3249 | merged | Release-3.4 backport of the DSACK correction; master implementation is !3235. |
| 3248 | merged | Cosmetic version-info placement of compiler details; no durable convention extracted. |
| 3247 | merged | Guy Harris bounds each pcapng block parser to the block data tvbuff, converts bounds failures into structural length diagnostics, validates trailing length, and updates tests. |
| 3246 | closed | Superseded attempt at the pcapng restructuring later merged as !3247; down-weighted. |
| 3245 | merged | Guy Harris release-3.2 backport fixing a copy/paste error in an expert-info field name. |
| 3244 | merged | Guy Harris release-3.4 backport fixing the same expert-info field-name error. |
| 3243 | merged | Guy Harris master fix for a copied expert-info field name; reinforces keeping registered diagnostic names semantically aligned. |
| 3242 | merged | RDP negotiation fix distinguishes a single selected protocol value from a bitmask of requested/supported protocols. |
| 3241 | merged | TShark gains TLS-session-key export with release-note integration; later file writing is centralized by !3260. |
| 3240 | merged | John Thacker raises an incorrectly small VLAN tag cap because legal nested encapsulations can exceed it; resource limits must admit valid protocol constructions. |
| 3239 | merged | DVB-S2-BB TS support. Pascal Quantin catches a nullable conversation access; John Thacker explains stream fragments must be added only on first pass or redissection asserts. |
| 3238 | merged | GTPv2 update to TS 29.274 V17.1.1; mostly protocol-table evolution. |
| 3237 | closed | I/O Graph border-color experiment with useful palette/dark-mode discussion, later redirected to newer work; down-weighted as unmerged. |
| 3236 | merged | Fixes misuse of tvb_memeql() return semantics and accepts both terminated and unterminated manufacturer IDs; API predicate semantics must be read exactly. |
| 3235 | merged | Successful master DSACK correction; authoritative successor to the contributor's closed attempts !3220, !3221 and !3234. |
| 3234 | closed | Superseded DSACK attempt. Pascal Quantin explains that commit-message repair can be done by amending the local commit and updating the topic branch rather than closing the MR. |
| 3233 | closed | NSH RFC 8393 attempt later implemented by !3837; superseded and down-weighted. |
| 3232 | merged | PROFINET DCP handles zero-length SET blocks as malformed while still decoding mandatory option/suboption/length framing. |
| 3231 | merged | Zigbee manufacturer-code registry update; no broader convention extracted. |
| 3230 | merged | Guy Harris release-3.2 backport of calculated 802.11 bitrate fallback. |
| 3229 | merged | Guy Harris release-3.4 backport of calculated 802.11 bitrate fallback. |
| 3228 | merged | 802.11 radio derives bitrate when metadata omits it. Guy Harris probes multi-user semantics before accepting the fallback assumption. |
| 3227 | merged | Gerald Combs adds a Debian Stable APT test job and carries the display-filter-list artifact into the older branch. |
| 3226 | merged | Gerald Combs moves display-filter-list generation to the CI job that owns the relevant packaged environment. |
| 3225 | merged | Release branch version bump 3.2.14 to 3.2.15; release mechanics only. |
| 3224 | merged | Release branch version bump 3.4.6 to 3.4.7; release mechanics only. |
| 3223 | merged | 3.2.14 release-build metadata update; release mechanics only. |
| 3222 | merged | 3.4.6 release-build metadata update; release mechanics only. |
| 3221 | closed | Early DSACK attempt closed after branch mistakes; superseded by !3235. |
| 3220 | closed | DSACK attempt with incorrect commit-history/commit-message repair. Pascal Quantin distinguishes MR-title edits from commit-message edits and recommends a clean single-commit topic branch. |
| 3219 | merged | SCCP adds an explicit opt-in preference for broken tracing tools that exceed DT1's standard length, preserving standards-compliant default behavior. |
| 3218 | merged | Exported-PDU comment typo correction; no broader convention extracted. |
| 3217 | merged | WSLua expert-info group reconciliation and PI_ASSUMPTION exposure; API consistency update. |
| 3216 | merged | Guy Harris release-3.2 backport of pcapng options-item extent fix. |
| 3215 | merged | Guy Harris release-3.4 backport of pcapng options-item extent fix. |
| 3214 | merged | Guy Harris sets the pcapng options parent item's end to the parser's actual stopping offset; readers accept both end-of-options-terminated and extent-terminated lists. |
| 3213 | merged | Gerald Combs migrates Windows help from CHM to HTML across build, installers, uninstall, runtime lookup and publication; follow-up review finds stale archive/link integration. |
| 3212 | merged | Guy Harris reorganizes 802.11 PV1 control/management information into protocol-consistent header/subtrees. |
| 3211 | merged | Guy Harris separates 802.11 PV0, PV1 and unknown-version parsing behind a small top-level version dispatcher and gives each version a proper protocol tree. |

Merged work is primary precedent. Closed !3246, !3237, !3234, !3233, !3221 and !3220 are down-weighted; merged successors are preferred where available. Guy Harris-authored work and direct Guy Harris technical review are given especially high evidentiary weight.
