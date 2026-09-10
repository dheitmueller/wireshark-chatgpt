# Wireshark MR Review Automation Ledger — !24509–!24558

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed MR set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and all available per-run files under `reviewed-mrs-automation/`, counting individual MR entries rather than assuming that a numeric range was wholly reviewed. The historical !17571–!17620 batch remains explicitly preserved and counted. The fifty highest-numbered corpus MRs absent from that reconstructed set were !24558 through !24509 inclusive; all fifty corpus files exist at the commit above.

| MR | Review depth | Outcome / durable evidence |
|---|---|---|
| !24558 | Deep | Merged John Thacker Heimdal Kerberos build/correctness repair. Prefer dependency public APIs such as `krb5_unparse_name()` over dereferencing implementation-specific principal internals; also ensures copied-key metadata length reflects the bytes actually copied. Strong API-boundary evidence. |
| !24557 | Deep | Merged HTTP/Megaco conversion to `tvb_get_string_uint*()`, removing temporary string allocation and `strtoul()`. Strong corroboration of bounded numeric parsing helpers. |
| !24556 | Deep | Merged John Thacker sync-pipe source move into the capture layer because only capture-child paths need it. Promoted as dependency-DAG/source-ownership guidance in `source-layout-conventions.md`. |
| !24555 | Scanned | Merged CMake nghttp2 dependency update. Routine dependency maintenance; no durable convention. |
| !24554 | Deep | Merged HTTP expert-info UTF-8 fix sanitizes packet-derived status-code text with `format_text()` rather than formatting arbitrary bytes directly; review also changes GLib booleans to standard C `bool`. Corroborates existing text-sanitization and C-type guidance. |
| !24553 | Scanned | Guy Harris-authored CMake comment correction describing code common to packet and event processors. Documentation/comment maintenance only. |
| !24552 | Deep | Merged John Thacker move of `cfile.h` under libwireshark/epan so installed header ownership matches its epan/wiretap/wsutil dependency layer. Promoted to `source-layout-conventions.md`. |
| !24551 | Deep | Merged release-4.6 backport fixing Netflix BBLog custom-block record length to match data actually materialized in the Buffer. Strong corroboration of existing Buffer logical-length invariants. |
| !24550 | Scanned | Merged Procmon wiretap bounds-check relocation keeps event-offset access inside the validated array domain. Bounds-safety corroboration only. |
| !24549 | Scanned | Merged pcapng change handles Custom Options for every block type that supports options rather than packet-only blocks. Format/spec compliance; no broader new rule. |
| !24548 | Deep | Merged John Thacker Buffer assertion enforces that logical length cannot be increased beyond allocated capacity. Strong invariant corroboration already covered by notebook guidance. |
| !24547 | Deep | Merged wiretap boundary check warns when record caplen exceeds Buffer length and defensively assures space while flagging the producer/module bug. Strong corroboration of Buffer metadata/materialization invariants. |
| !24546 | Scanned | Merged RTPS change enabling DATA_FRAG reassembly by default, accompanied by sample capture evidence. Product/protocol behavior change; no general convention promoted. |
| !24545 | Deep | Merged John Thacker master fix for Netflix BBLog custom-block length; record metadata is changed to describe the bytes actually placed in the Buffer, preventing stale-data exposure/overflow. Existing Buffer invariant corroboration. |
| !24544 | Scanned | Merged Lua Debugger value-highlighting/UI enhancement. UI-specific behavior; no durable cross-cutting rule. |
| !24543 | Deep | Merged John Thacker sharkd fix moves cumulative-byte state from function-static storage into `capture_file` and resets it on file open. Promoted to `state-modeling-conventions.md`. |
| !24542 | Scanned | Merged dissector refactor separating additional fetch/display operations. Useful cleanup but already covered by existing fetch-once/display-helper conventions. |
| !24541 | Scanned | Merged release-4.6 GitLab CI path update. Backport/CI maintenance only. |
| !24540 | Scanned | Merged master GitLab CI path update. CI maintenance only. |
| !24539 | Scanned | Merged rpm-setup shell typo/syntax repair. Local tooling fix; no new convention. |
| !24538 | Deep | Merged John Thacker build-warning change prefers `-Wstack-usage` over `-Wframe-larger-than` because it provides a more conservative upper-bound model and accounts for dynamic stack use. Useful compiler-diagnostic selection evidence; retained without a separate notebook rule. |
| !24537 | Scanned | Merged release-4.4 manual macOS CI backport. Release CI maintenance only. |
| !24536 | Scanned | Merged release-4.6 macOS CI updates and narrower ccache reporting scope. Useful job-scope hygiene but no distinct convention promoted. |
| !24535 | Deep | Merged John Thacker DNP3 fix validates minimum protocol length before subtracting header bytes, using checked arithmetic and expert diagnostics. Strong corroboration of existing validation-before-arithmetic guidance. |
| !24534 | Scanned | Merged release-4.6 LDAP NULL-key guard before hash lookup. Defensive API-precondition backport. |
| !24533 | Scanned | Merged release-4.6 HTTP NULL-key guard before hash lookup. Same defensive API-precondition evidence. |
| !24532 | Scanned | Merged release-4.6 libmaxminddb dependency update. Routine maintenance. |
| !24531 | Scanned | Merged rpm-setup change avoids installing unsupported asciidoctor on SUSE 15 and leaves optional documentation dependencies to the user. Platform packaging compatibility only. |
| !24530 | Scanned | Merged release-4.4 OpenCore-AMR SUSE package-name backport. Packaging maintenance only. |
| !24529 | Scanned | Merged release-4.6 OpenCore-AMR SUSE package-name backport. Packaging maintenance only. |
| !24528 | Deep | Merged John Thacker memcache conversion to `ws_buftou*()` removes temporary C strings and fixes platform-width truncation where Windows `long` is 32-bit. Promoted as bounded width-specific numeric parsing evidence. |
| !24527 | Scanned | Merged Lua Debugger migration to Qt model/view. Useful UI architecture example, but no new cross-cutting convention. |
| !24526 | Deep | Merged master HTTP guard prevents passing a NULL header name into a hash function that requires a non-NULL key. Defensive API-precondition evidence; backported by !24533. |
| !24525 | Deep | Merged master LDAP guard prevents passing a NULL attribute type into a hash function that requires a non-NULL key. Defensive API-precondition evidence; backported by !24534. |
| !24524 | Deep | Merged John Thacker `tvb_get_string_uint*()` API addition parses bounded numeric text directly from TVBs, avoiding allocation/copy/NUL termination and platform-width ambiguity. Promoted to `bounded-text-numeric-parsing-conventions.md`. |
| !24523 | Scanned | Merged documentation removal of references to obsolete `tvb_get_nstringz*` APIs. Documentation/API synchronization only. |
| !24522 | Down-weighted | Closed, unmerged TCAP/GSMMAP GlobalValue decoding proposal. Anders Broman repeatedly concluded the sample/sender was malformed and the proposed dissector change was wrong; failed pipeline and unmerged outcome mean it is rejection evidence, not accepted architecture. |
| !24521 | Scanned | Merged release-4.4 NAS 5GS copy/paste fix selects the correct S-NSSAI location-validity IE decoder. Backport of !24519. |
| !24520 | Scanned | Merged release-4.6 NAS 5GS copy/paste fix selects the correct S-NSSAI location-validity IE decoder. Backport of !24519. |
| !24519 | Scanned | Merged master NAS 5GS correction fixes an incorrect IE decoder selected by copy/paste. Local protocol correctness fix. |
| !24518 | Deep | Merged John Thacker `ws_buftou8/16/32`-style counted-string helpers and conversions. Directly parses bounded byte spans without manufacturing C strings; promoted to `bounded-text-numeric-parsing-conventions.md`. |
| !24517 | Deep | Merged release-4.4 backport of counted-string `ws_strtoi`/`ws_buftou*()` work, including tests for non-NUL-terminated/internal-NUL buffers. Strong corroboration of bounded parsing semantics. |
| !24516 | Deep | Merged release-4.4 sharkd exception-handling backport catches TVB bounds and dissector errors around data-source extraction so malformed dissection cannot leave invalid JSON or crash the server. Corroborates existing TVB/error-boundary guidance. |
| !24515 | Scanned | Merged GitLab CI macOS prerequisite update. CI environment maintenance; no durable convention. |
| !24514 | Scanned | Merged release-4.6 macOS package-build update. Backport/build maintenance only. |
| !24513 | Deep | Merged release-4.6 backport of counted-string numeric parsing, preserving direct bounded-buffer semantics and test coverage. Corroborates !24518/!24524. |
| !24512 | Scanned | Merged release-4.6 Windows Qt 6.10.3 update while retaining an older macOS Qt because 6.10 dropped macOS 12 support. Useful platform support-matrix evidence; no new generic rule. |
| !24511 | Scanned | Merged master macOS Python/build-prerequisite CI update. CI maintenance. |
| !24510 | Scanned | Merged libmaxminddb 1.13.3 dependency update. Routine dependency maintenance. |
| !24509 | Deep | Merged John Thacker sharkd fix broadens data-source extraction handling from only `DissectorError` to the full bounds/dissector-error family, preserving valid JSON under malformed/truncated data. Strong error-boundary corroboration. |

## Promoted durable conventions

- !24552 and !24556: source/header placement should follow the actual ownership and dependency DAG. Put interfaces at the lowest layer that owns their dependencies, and implementation in the subsystem that actually consumes it; do not let nominally generic locations create upward or unnecessary dependencies.
- !24543: mutable state that restarts when a capture/session is opened belongs on that owning session object and must be reset at its lifecycle boundary, rather than living in process-static storage.
- !24518, !24524, !24528, !24557, with backports !24517/!24513: parse bounded numeric text directly with width-specific Wireshark buffer/TVB helpers instead of allocating temporary NUL-terminated strings for `strtoul()`-style APIs. This preserves explicit bounds and numeric width and avoids Windows `long` truncation.

Strong merged work in !24545/!24547/!24548/!24551 (Buffer logical-length/capacity invariants), !24554 (packet-text sanitization), !24535 (validation before arithmetic), and !24516/!24509 (TVB exception containment) was intentionally treated as corroboration of conventions already present rather than duplicated. Closed !24522 was explicitly down-weighted and not treated as accepted behavior.