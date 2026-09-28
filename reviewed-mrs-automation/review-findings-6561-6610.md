# Review findings: Wireshark MRs !6561-!6610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Evidence policy: merged master changes and substantive maintainer review are weighted most heavily. Stable backports mainly corroborate master behavior. Closed !6578 is retained only as workflow evidence.

| MR | Outcome | Depth | Findings |
|---|---|---|---|
| !6610 | merged | Scanned | Gerald Combs; make Sinsp discovery depend on BUILD_logwolf rather than NOT WIN32. Optional dependency probing should follow the feature or target that consumes it, not an incidental platform predicate. |
| !6609 | merged | Scanned | Use a strict C prototype with (void) for a no-argument function; small declaration-correctness cleanup. |
| !6608 | merged | Deep | João Valverde; lexer work lets arithmetic operators be adjacent to operands while adding explicit MAC, IP, and CIDR token patterns plus a regression test. Operator-token changes must preserve literal tokenization. |
| !6607 | merged | Scanned | Splits singleton initialization locking from long-running RTP processing locking to avoid self-deadlock when the same dialog is opened twice. |
| !6606 | merged | Scanned | Adds the distro jsoncpp include path to Sinsp discovery; build portability maintenance. |
| !6605 | merged | Scanned | Removes stale commented Qt build-warning code; no broader convention. |
| !6604 | merged | Scanned | Logwolf quick-start documentation cleanup; no new engineering rule. |
| !6603 | merged | Discussion-focused | John Thacker asks contributors to put the issue number in the commit message so closure automation works and to attach a capture demonstrating dissector bugs to the issue. Richard Sharpe confirms the offset fix. |
| !6602 | merged | Scanned | Renames the Wireshark plugin-library macro while keeping a warning compatibility wrapper for the old name and migrating in-tree callers. Useful deprecation pattern. |
| !6601 | merged | Deep | Alexis flags prohibited sprintf; John Thacker identifies the real API bug: val_to_str_ext() accepts a formatting fallback whereas val_to_str_ext_const() uses a literal fallback. Choose the semantic helper instead of adding ad-hoc formatting. |
| !6600 | merged | Scanned | Migrates dissectors and wiretap debug paths from GLib logging to Wireshark logging domains and helpers. |
| !6599 | merged | Scanned | Rewrites built-in color filters using newer display-filter set and range syntax; exercises language compatibility of shipped filters. |
| !6598 | merged | Deep | João Valverde changes logical precedence so AND binds tighter than OR and updates grammar, release notes, filter documentation, and examples together. User-visible language precedence is an API contract. |
| !6597 | merged | Scanned | Allows an exact 0bXXXXXXXX literal to become a one-byte array despite ambiguity with separator-free byte strings; documents the ambiguity explicitly. |
| !6596 | merged | Scanned | Stops RLC-NR from overwriting SDAP configuration learned from RRC with a default zero. Lower layers should not clobber already-established higher-layer state with an assumed default. |
| !6595 | merged | Deep / high-authority | Authored by Guy Harris. Interface statistics update every underlying interface, so iteration uses source_model_.rowCount() even though indices are later mapped through proxy and view models. Model traversal must follow operation semantics. |
| !6594 | merged | Scanned | Stable-branch cherry-pick of the hidden-interface statistics fix; corroborates !6580 and !6595. |
| !6593 | merged | Scanned | Automatic registry, data, and release-note update; no durable review convention. |
| !6592 | merged | Scanned | Automatic registry and data update; no durable review convention. |
| !6591 | merged | Scanned | Automatic registry and data update; no durable review convention. |
| !6590 | merged | Scanned | PROFINET separates input and output IO-data and IOCS counts and aggregates per-API counts into the CR total; protocol-specific state correction. |
| !6589 | merged | Scanned | Adds PROFIsafe 2.6 five-byte safety-trailer support alongside the older four-byte trailer; protocol-version handling. |
| !6588 | merged | Scanned | CoAP follow-up switches column and item text to format_text_string(), reinforcing that packet-derived text shown in UI summaries should pass through Wireshark text formatting and sanitization helpers. |
| !6587 | merged | Deep / high-authority | Reverts the Skinny generated-output change because the canonical generator path was broken under Python 3. Guy Harris reproduces the failure on macOS with Python 3.8 and points to Python3-ifying the generator; John notes later !8777. |
| !6586 | merged | Scanned | Adds ccache to the Debian optional package list; developer-build convenience. |
| !6585 | merged | Discussion-focused | macOS setup accepts Xcode command-line tools plus qmake. Roland Knall explicitly asks for x86 and Apple Silicon coverage and supplies the architecture the author could not test. Platform build changes should cover each supported architecture. |
| !6584 | merged | Scanned | John Thacker adds a heuristic TPKT path for RDP so mixed transport and security modes can be recognized without over-claiming the port. |
| !6583 | merged | Scanned | RDP and TLS registration avoids ssl_dissector_add because that helper would also force TLS as the TCP dissector on 3389, while RDP may be TLS or direct TCP. Register at the layer actually being extended. |
| !6582 | merged | Scanned | Same RDP TLS-subdissector correction on another maintained branch; duplicate implementation evidence. |
| !6581 | merged | Scanned | Same RDP TLS-subdissector correction; duplicate and backport evidence. |
| !6580 | merged | Deep | Master fix for hidden-interface statistics: iterate the source model because hidden rows still need statistics refreshed, then map through proxies as needed. |
| !6579 | merged | Scanned | Corrects ZBNCP display-filter abbreviations; compatibility-facing naming cleanup. |
| !6578 | closed | Discussion-focused / negative | Alexis La Goutte asks for the fix on master first; contributor opens !6580 on master. Guy Harris later explains !6594 is the stable cherry-pick of that master change. Closed stable-first submission is workflow evidence, not implementation precedent. |
| !6577 | merged | Scanned | Additional display-filter arithmetic semantic-check fixes, including clearer rejection of constant-only LHS expressions and targeted tests. |
| !6576 | merged | Scanned | CIP presents Attribute ID in decimal while retaining other segment display behavior; protocol presentation detail. |
| !6575 | merged | Deep | Completes arithmetic-on-LHS support and rejects constant-only LHS forms; merged follow-up to user-reported modulo use from !6568. |
| !6574 | merged | Discussion-focused | CoAP applies packet-text formatting helpers. Coverity reports a leak because it assumes pinfo->pool can be NULL; Stig and John explain the returned string is pool-owned. Treat analyzer findings against scoped allocators semantically, not mechanically. |
| !6573 | merged | Scanned | HTTP/2 workaround sizes dummy entries from nghttp2's actual configured maximum dynamic-table size instead of hard-coded 4096. Runtime and library state should replace protocol-default assumptions where configuration can vary. |
| !6572 | merged | Scanned | Corrects CIP epoch constant and date conversion; protocol-specific time fix. |
| !6571 | merged | Scanned | Documents new display-filter bitwise and arithmetic syntax and release-note wording; no separate rule beyond the language-change series. |
| !6570 | merged | Scanned | CentOS 7 build fix uses explicit-width psnip_safe_int32 and int64 helpers instead of generic forms unavailable there. Portability code should use the capability actually supported by the minimum toolchain. |
| !6569 | merged | Scanned | Clarifies selected-frame wording in a display-filter reference comment. |
| !6568 | merged | Deep | João Valverde adds multiply, divide, and modulo across scanner, grammar, semantic checks, ftypes, VM, docs, and tests; user feedback exposed missing LHS support, fixed in !6575. Language features need end-to-end and usage-driven tests. |
| !6567 | merged | Deep | John Thacker reworks TCP out-of-order dissection to queue missing segments, resume as soon as sequence continuity permits, preserve each fragment's original frame through a new reassembly API, and keep first-pass and redissection results consistent. |
| !6566 | merged | Scanned | Adds a missing exported symbol to Debian packaging metadata; ABI packaging maintenance. |
| !6565 | merged | Scanned | RTP Analysis explicitly refreshes statistics after retap processing before removing listeners; UI derived state must be updated after the data pass that populates it. |
| !6564 | merged then superseded | Deep / negative | Jörg Mayer catches a direct edit to generated packet-skinny.c. Martin discovers the generated-file detector is case-sensitive against the file's actual header and cannot regenerate because the generator path is broken; !6587 reverts it. Fix source and generator tooling, not generated output. |
| !6563 | merged | Scanned | Adds NNTP STARTTLS conversation state and hands off to TLS after the protocol transition; useful protocol-state pattern but little review discussion. |
| !6562 | merged | Deep | Introduces binary add and subtract through display-filter AST, semantic checking, ftype operations, VM, release notes, and tests; foundation for the subsequent arithmetic series. |
| !6561 | merged | Scanned | Minor release-note cleanup for display-filter syntax changes. |

Validation: 50 rows, 50 unique MRs, minimum !6561, maximum !6610.
