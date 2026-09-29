# Wireshark MR review findings 5261-5310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 MRs were reviewed. Merged master work is primary evidence; stable backports corroborate; closed work is lower-weight history.

| MR | State | Weight | Finding |
|---|---|---|---|
| !5310 | merged | Primary | F1AP adds more RRC-Container dispatch in the ASN.1 `.cnf` source and regenerated dissector output. Straightforward generated-dissector extension; reinforces authoritative-input plus regeneration practice. |
| !5309 | merged | Primary | Rewrites `format_text_chr()` around `wmem_strbuf` instead of private buffer macros. Useful shared-API cleanup; no new policy beyond preferring existing memory/string abstractions. |
| !5308 | merged | Primary, later semantic correction | Converts ASN.1 GeneralizedTime fields to `FT_ABSOLUTE_TIME` using the common ISO-8601 parser and generator path. Good semantic field typing; timezone-sign expectations inherited from the then-current parser were later corrected by !5668. |
| !5307 | merged | Primary | Moves a name-resolution command-line test from live capture to a capture file because live capture was unnecessary and flaky on macOS Arm. Tests should avoid environmental dependencies unrelated to the contract under test. |
| !5306 | closed | Low | Draft Asterix raw-value fix touched generator input and generated output; Alexis requested test coverage. Closed/unmerged, so no implementation precedent. |
| !5305 | merged | Stable corroboration | Release-3.6 backport of the IPsec NULL-heuristic display correction in !5304. |
| !5304 | merged | Primary | Restores correct ESP padding, next-protocol, and ICV presentation when the NULL-encryption heuristic is used. Focused dissector correctness fix. |
| !5303 | merged | Primary with superseded defects | Large SSH decryption architecture series. Jörg Mayer required clean independently buildable commits, fixups folded into the commit that introduced the problem, focused changes, and representative captures/instructions. Later review found redissection and platform/dependency bugs; !5801 and follow-ups are authoritative for those corrected semantics. John Thacker also required coordination of the new pcapng DSB secrets type with the pcapng registry/specification. |
| !5302 | merged | Primary | Stops display-filter diagnostic/compiled representations from mangling valid UTF-8 into byte escapes. Escape syntax-significant characters, not valid UTF-8 merely because bytes are non-ASCII. |
| !5301 | merged | Primary, strong review | New ZBOSS dissector. Jaap Keuter corrected `FT_BOOLEAN` display semantics and function-static mutable dissector state, directing stream state to conversations. Gerald Combs requested natural-width loop counters instead of packet-width `guint8` counters to avoid future overflow/loop hazards. Review also caught NULL-terminated option-list and duplicate-field-registration issues. |
| !5300 | merged | Stable, Guy-authored | Guy Harris-authored release-3.6 backport of the pcapng option-accounting underflow fix from !5288. |
| !5299 | merged | Primary, later semantic correction | Adds `ENC_ISO_8601_DATE_TIME_BASIC` and tests. The test vectors shared the timezone-offset sign misconception later fixed by !5668; useful negative evidence for deriving sign-sensitive expected values independently. |
| !5298 | merged | Primary, later semantic correction | Adds explicit basic/extended ISO-8601 parser modes. The timezone-offset sign arithmetic in this historical version was later corrected by !5668; the old sign behavior is not current convention. |
| !5297 | merged | Primary | Removes a now-unneeded macOS notarization serialization wait after tool behavior changed. Packaging maintenance; no broad convention promoted. |
| !5296 | merged | Stable/parallel corroboration | Same notarization-wait removal on another maintained line. |
| !5295 | merged | Primary, Guy review | Adds Capture Options extcap configuration UI. Guy Harris compared Welcome-screen and Capture Options double-click behavior across macOS and Ubuntu, driving parity of the intended interaction contract rather than merely matching icons. |
| !5294 | merged | Stable/parallel corroboration | Same notarization-wait removal on another maintained line. |
| !5293 | merged | Primary | Mechanical display-filter grammar indentation/editorconfig cleanup; no semantic rule. |
| !5292 | merged | Stable corroboration | Backport of TECMP LIN off-by-one payload fix. |
| !5291 | closed | Low, superseded | Extcap logging redesign accumulated broad discussion but was superseded by accepted !5311, !5326, and !5357 work. Retained only as design-history evidence. |
| !5290 | merged | Primary | Avoids duplicate allocation/copy when saving display-filter token values. Internal ownership cleanup. |
| !5289 | closed | Low | Asterix generator-only fix. Alexis asked that regenerated output be updated simultaneously; Graham Bloice advised amending the same MR rather than closing/reopening because MR churn loses review context. |
| !5288 | merged | Primary | Fixes pcapng option-processing underflow by accounting for the rounded end-of-options record length before the common decrement. Cursor/accounting state must reflect bytes the enclosing loop will still subtract. |
| !5287 | merged | Primary | Stores lexical token spelling in the display-filter syntax tree so diagnostics can echo `&&` when the user typed `&&` rather than rewriting it as `and`. Preserve source lexemes separately from canonical semantic operators when diagnostics need source fidelity. |
| !5286 | merged | Primary | Adds DRBD packet support. Review favors coherent small commits in one MR without over-fragmenting trivial changes. |
| !5285 | merged | Primary | Master origin of TECMP LIN off-by-one correction; author explicitly identified need for master and release-3.6 coverage. |
| !5284 | merged | Primary | Adds release build configuration to version information. Straightforward diagnostics/build metadata. |
| !5283 | merged | Primary, Guy/Jaap review | Removes obsolete `STR_ASCII`/`STR_UNICODE` display values. Guy Harris explains that the `header_field_info` member is `display`, while “base” is a historical numerical-field artifact; Jaap Keuter's generic `BASE_NONE` direction is accepted. |
| !5282 | merged | Stable corroboration | Release-3.4 backport of BT-DHT endless-loop fix !5280. |
| !5281 | merged | Stable corroboration | Release-3.6 backport of BT-DHT endless-loop fix !5280. |
| !5280 | merged | Primary | Fixes BT-DHT endless loop by returning failure `0` on structurally invalid compact-node length instead of a plausible positive “remaining length” that the caller treated as successful consumption. |
| !5279 | merged | Primary | COSE uses a protocol-in-name-only registration for nested headers so `frame.protocols` is not polluted with repeated `:cose` entries. Presentation/registration choice should reflect protocol-stack semantics. |
| !5278 | merged | Primary | Removes long-unused GTK-era `format_uri()` and exported symbol. Dead API cleanup. |
| !5277 | closed | Low | Parser-tracing environment-variable proposal closed by its author as not worth the added complexity. No accepted precedent. |
| !5276 | merged | Primary | macOS Extras package metadata declares arm64/x86_64 host architectures. Packaging compatibility only. |
| !5275 | merged | Stable/parallel corroboration | Same macOS Extras host-architecture metadata on another line. |
| !5274 | merged | Primary | Display-filter internal function cleanup; no new durable rule. |
| !5273 | merged | Primary | Removes duplication from `format_text_wsp()`; localized implementation cleanup. |
| !5272 | merged | Primary | Adds rationale comment for `format_text_chr()`; documents non-obvious helper intent. |
| !5271 | merged | Stable/parallel corroboration | Same macOS Extras host-architecture metadata on another maintained line. |
| !5270 | merged | Primary | Increases proto field-registration preallocation. Performance/tuning change, no general convention extracted. |
| !5269 | merged | Primary | Adds Doxygen markers to UI headers. Mechanical documentation coverage. |
| !5268 | merged | Primary | Adds Doxygen markers to remaining headers; when CI flagged prohibited APIs in an unrelated candidate header, the contributor removed that file from this MR rather than declaring the failing pipeline irrelevant. |
| !5267 | merged | Primary | Adds Doxygen markers to epan headers. Mechanical documentation coverage. |
| !5266 | merged | Primary | Adds Doxygen markers to extcap headers. Mechanical documentation coverage. |
| !5265 | merged | Primary | Adds Doxygen markers to wsutil headers. Mechanical documentation coverage. |
| !5264 | merged | Primary | Adds Doxygen markers to capture headers and synchronizes Doxygen directory coverage. |
| !5263 | merged | Primary | Adds Doxygen markers to wiretap headers. Mechanical documentation coverage. |
| !5262 | merged | Primary | Adds Doxygen markers to exported public headers. Mechanical documentation coverage. |
| !5261 | merged | Primary, substantial review | Extends ciscodump for IOS XE/ASA. Windows CI exposed POSIX-only APIs and narrowing assumptions; Dario Lombardo directed use of project conversion/network helpers, clean commit history, concise logging, warnings for unsupported devices, and independent real-device testing of configuration cleanup, early stop, load, and capture behavior. |

## Batch conclusions

The strongest new durable material is promoted into the topical notebook files and summarized in `conventions-5261-5310.md`. Historical behavior superseded by later accepted fixes—especially ISO-8601 timezone signs and the SSH redissection bug—is recorded as provenance, not current convention.
