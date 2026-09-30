# Review findings: MRs 4311–4360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master changes are primary evidence. Stable-branch backports are corroborating evidence, and closed/unmerged work is explicitly down-weighted. Direct maintainer review is weighted by authority and whether the guidance was incorporated or otherwise corroborated.

| MR | Outcome / weight | Author | Finding |
|---|---|---|---|
| 4360 | merged, master | Gerald Combs | Packaging/CI accommodates Asciidoctor where distro packages differ; Gerald also notes that losing Docker Hub automated rebuilds is a reason to prefer the project container registry. Useful build-environment evidence, but no new core code convention. |
| 4359 | merged, master | João Valverde | Moves display-filter function recognition out of the scanner and into grammar/parser construction: an unparsed identifier followed by parentheses is resolved as a function there, with parser-specific diagnostics. Keeps lexical recognition simpler and lets syntax context drive meaning. |
| 4358 | merged, master | Gerald Combs | Man-page/Asciidoctor cleanup and removal of obsolete POD conversion tooling. Documentation maintenance; no new durable code rule. |
| 4357 | merged, master | João Valverde | Renames grammar symbols to describe their grammar role rather than incidental C implementation names. Readability cleanup; no separate convention promoted. |
| 4356 | merged, master | João Valverde | Improves display-filter diagnostics by retaining token text in syntax-tree nodes and adding structured string representations/logging. Corroborates diagnostic-state and parser-debugging practices. |
| 4355 | merged, master | Pascal Quantin | NR-RRC ASN.1 update to 3GPP 38.331 v16.6.0 and regeneration. Protocol-spec maintenance. |
| 4354 | merged, master | Pascal Quantin | LTE-RRC ASN.1 update to 3GPP 36.331 v16.6.0 and regeneration. Protocol-spec maintenance. |
| 4353 | merged, master | Stig Bjørlykke | Makes Lua FileHandler seek_read optional by providing seek+read fallback. Guy Harris gives authoritative review that EOF on random-access reread should become WTAP_ERR_SHORT_READ, and suggests normalizing that contract at the top Wiretap layer so C and Lua readers share it. |
| 4352 | merged, master | Stig Bjørlykke | Capture-info processing follows whether the Wiretap resource was actually initialized rather than the mutable show_info preference. Corroborates the stronger lifecycle rule already represented by later reviewed MRs 4362/4361. |
| 4351 | merged, master | Alexis La Goutte | Clang/static-analysis cleanup in COSE, including prototypes. Tooling hygiene; no distinct new rule. |
| 4350 | merged, release-3.4 | Joakim Andersson | Stable backport of Nordic BLE address-resolved flag support from MR 4338. Corroborating branch evidence only. |
| 4349 | merged, master | João Valverde | Fixes syntax-tree debug display and initializes node value state. Small correctness follow-up to display-filter diagnostic work. |
| 4348 | merged, master | João Valverde | Replaces a dedicated syntax-tree boolean with a flags field, preserving the inside-parentheses semantic while making node state extensible. |
| 4347 | merged, master | João Valverde | Moves deprecated-token tracking into display-filter work state and adds regression tests. Useful evidence that deprecation reporting is parser/compiler state, not incidental AST-node state. |
| 4346 | merged, master | Adrian Granados | Adds 6 GHz 802.11 frequency-to-channel conversion; review references existing header/PHY issues. Protocol-specific mapping change. |
| 4345 | merged, master | João Valverde | Moves display-filter debug output to runtime-controllable wslog and adds canonical syntax-tree textual representations. Corroborates structured diagnostics rather than ad-hoc printf debugging. |
| 4344 | merged, master | John Thacker | Bounds weak CAM Inspector/VeriWave file probes instead of scanning arbitrarily large candidates and fixes overflow-prone heuristic counting. Guy Harris explicitly says even the accepted 1 GiB bound could probably be reduced to roughly 1–16 MiB; speculative format detection needs a conservative work budget. |
| 4343 | merged, release-3.4 | John Thacker | Stable backport of the absolute-time display-filter representation fix from MR 4318. |
| 4342 | merged, master | João Valverde | Adds ws_getopt regression coverage for optional arguments. Focused API test. |
| 4341 | merged, master | João Valverde | Renames public getopt compatibility types/macros to ws_option/ws_*_argument. Public/project API identifiers should be namespaced instead of colliding with libc/system names. |
| 4340 | merged, master | João Valverde | Adds singular/plural aliases for log-domain option/environment naming. CLI compatibility/usability adjustment. |
| 4339 | merged, master | Martin Mathieson | Makes COSE helpers translation-unit-local. Ordinary linkage hygiene. |
| 4338 | merged, master | Joakim Andersson | Master-origin Nordic BLE address-resolved flag support. Protocol-specific metadata addition. |
| 4337 | merged, master | John Thacker | Defers expensive capinfos hashing until Wiretap has established that the input is a capture format. Guy Harris probes behavior for captures that open but later have read errors; the broader partial-error-reporting redesign remains out of scope. |
| 4336 | merged, master | Gerald Combs | Windows dependency updates only. |
| 4335 | merged, master | Jaap Keuter | Registers Ethernet over IPv6 per RFC 8986. Protocol dispatch maintenance. |
| 4334 | merged, master | Evan Huus | CBOR helper uses explicit pinfo->pool rather than ambient wmem_packet_scope(). Strong corroboration of explicit allocator-scope guidance. |
| 4333 | merged, master | Jaap Keuter | Determines Prefix-SID representation from the sub-TLV length and validates V/L flags separately. Malformed flags no longer suppress bytes whose structural format is clear from length. |
| 4332 | merged, release-3.4 | Stig Bjørlykke | Stable backport of dynamic heuristic-registration lifetime fix from master MR 4324. |
| 4331 | merged, master | Joakim Karlsson | Marks locals volatile where Wireshark exception/longjmp control flow can otherwise trigger clobber analysis. Useful C control-flow portability evidence. |
| 4330 | merged, master | João Valverde | Makes dftest filter output less visually ambiguous by dropping unnecessary surrounding quotes. |
| 4329 | merged, master | Thomas Dreibholz | Adds protocol message type to Info columns and removes duplicate lookup. Local dissector presentation cleanup. |
| 4328 | merged, master | Joakim Karlsson | Extends JSON binary-data lookup into arrays. Protocol feature change. |
| 4327 | merged, master | Pascal Quantin | LPP ASN.1 update to 3GPP 37.355 v16.6.0 and regeneration. |
| 4326 | merged, master | Evan Huus | GUID lookup/string APIs accept explicit allocator scopes; packet callers use pinfo->pool. The stated goal is to avoid the global unprotected packet pool and let the compiler make lifetime/context dependencies explicit. |
| 4325 | merged, master | Adrian Granados | Adds Ruckus vendor-specific 802.11 IE dissection. Protocol-specific extension. |
| 4324 | merged, master | Stig Bjørlykke | Master-origin dynamic-registration lifetime fix: heuristic entries are logically deregistered immediately but backing storage is deferred because already-dissected UDP packets can retain pointers until redissection/cleanup. Direct reproducer confirmation appears in discussion. |
| 4323 | merged, master | John Thacker | Documents absolute-time display-filter syntax/quirks. Documentation corroboration for MR 4318. |
| 4322 | merged, master | Martin Mathieson | Spelling cleanup also renames a registered ISUP filter abbreviation. Treat as historical counterevidence only: later stronger review establishes registered filter names as compatibility surfaces. |
| 4321 | merged, master | Anders Broman | GSM MAP ASN.1 update to 3GPP 29.002 v17.1.0. Protocol-spec maintenance. |
| 4320 | merged, master | Joakim Karlsson | Extends JSON binary-data lookup to non-compact tree presentation. Protocol feature change. |
| 4319 | merged, master | João Valverde | Adds explicit project architecture guidance: epan must not depend on epan/dissectors; dissectors are clients of epan APIs, with runtime registration providing the inversion needed for plugins, on-demand loading, and testability. |
| 4318 | merged, master | John Thacker | Makes FT_ABSOLUTE_TIME display-filter text use the same local-time semantics accepted by the parser so generated filters round-trip. Master-origin of stable MR 4343. |
| 4317 | merged, master | Gerald Combs | POD markup cleanup only. |
| 4316 | merged, master | Anders Broman | Adds GSM MAP noteSubscriberPresent dissection. Protocol-specific extension. |
| 4315 | merged, master | Berk Akinci | Tomasz Moń asks to reuse the proto_item returned by the existing field-add call and append the extra displayed integer, instead of duplicating nearly identical add/format logic. Contributor adopts it. |
| 4314 | merged, master | João Valverde | Removes a CMake deb-package target described as unreliable with Ninja and build-tree-polluting; Debian packages should be built from a source tarball. Useful packaging-boundary guidance. |
| 4313 | closed, low weight | João Valverde | Large display-filter grammar/error-reporting work was closed and superseded by smaller merged follow-ups such as MRs 4359/4356/4347. Review-history evidence only, not implementation precedent. |
| 4312 | merged, master | John Thacker | Before tcp_dissect_pdus(), SMPP validates that the current captured boundary plausibly begins an SMPP PDU; otherwise it returns 0. This avoids bogus length/reassembly state when a capture starts mid-TCP stream; successful heuristic recognition then binds the conversation. |
| 4311 | merged, master | Stig Bjørlykke | Lua FileHandler reload uses actual loaded-reader state, reloads the file, and delays dynamic cleanup until redissection. Discussion catches and removes a double-redissect path; a separate heuristic crash is traced to and fixed by master MR 4324. |

## Maintainer weighting

Guy Harris provides substantive review in three MRs. In MR 4353 he states that an EOF result on random-access reread is semantically different from sequential EOF and should be surfaced as `WTAP_ERR_SHORT_READ`, preferably at the shared Wiretap layer. In MR 4344 he questions the scale of the historical 1 GiB probe bound and suggests a much smaller 1–16 MiB budget. In MR 4337 he explicitly checks the boundary between capture-format recognition, later read failures, and whether a hash remains meaningful/reportable. These comments receive high review weight, while the notebook distinguishes recommendations from behavior actually incorporated in the specific merge.

Other strong evidence includes João Valverde's merged architecture/API/parser work (MRs 4359, 4341, 4319), Stig Bjørlykke's merged dynamic-registration and reload fixes (MRs 4324, 4311), John Thacker's merged stream/file-detection work (MRs 4344, 4337, 4312), Jaap Keuter's length-versus-flags parser correction (MR 4333), and Tomasz Moń's direct field-construction review in MR 4315.
