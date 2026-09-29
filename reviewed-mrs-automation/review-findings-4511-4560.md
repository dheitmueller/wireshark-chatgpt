# Wireshark MR review findings — 4511–4560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Weighting: merged master work is primary evidence; maintained-branch backports corroborate their master origins; closed or superseded submissions are lower-weight history. No substantive Guy Harris authored change or review comment appeared in this batch, so no Guy-derived convention is inferred.

| MR | Outcome | Review finding |
|---|---|---|
| !4560 | Merged | Gerald Combs continues the Qt migration from string-based `SIGNAL()/SLOT()` wiring to typed member-pointer connections, giving compile-time signal/slot checking. Later notebook evidence already records this convention more strongly. |
| !4559 | Merged | Spelling cleanup also renames several registered field abbreviations. This is historical merged counterevidence only; later, stronger review establishes field abbreviations as compatibility surfaces, so spelling alone is not a general license to rename them. |
| !4558 | Merged | Automatic registry/translation update on master. Generated-data maintenance only; no new durable rule beyond existing generated-source conventions. |
| !4557 | Closed | Automatic update for release-3.6 was closed. Down-weighted; no implementation precedent. |
| !4556 | Merged | João Valverde replaces six independent ftype comparison callbacks with one memcmp-style ordering callback whose sign encodes less/equal/greater, reducing duplicated relation semantics. |
| !4555 | Merged | Gerald Combs reverts the newly added weekly "Update Numbers" CI job. Together with !4531 this is supersession evidence that repository-mutating automation should not be treated as established merely because its first implementation merged. |
| !4554 | Merged | Debian packaging was updated toward distro packaging, but Anders Broman subsequently reported that packaging failed on Ubuntu 18.04. Baseline/version assumptions need validation on the oldest supported build environment. |
| !4553 | Closed | Superseded Debian-packaging submission with the wrong source branch; the author points to corrected successor !4554. |
| !4552 | Merged | TECMP FlexRay null frames may carry bytes but semantically contain invalid data; the accepted fix gates child dissection on the Null Frame flag rather than payload length alone. |
| !4551 | Merged | Corrects dynamically generated AUTOSAR-NM field abbreviations after a protocol rename. Useful maintenance history, but later compatibility guidance limits what can safely be generalized from renaming registered filters. |
| !4550 | Merged | Automatic registry/release-note update. Routine generated-data maintenance. |
| !4549 | Merged | Automatic release-3.4 generated documentation/translation update. Routine maintenance. |
| !4548 | Merged | Automatic Debian translation update. Routine generated-data maintenance. |
| !4547 | Merged | TCP analysis restores sequence state after a failed connection attempt to avoid cascaded false retransmissions. John Thacker questioned setting the reused-port flag when the base sequence matches, so the narrow merged behavior is stronger evidence than any broader inference. |
| !4546 | Merged | Gerald Combs fixes test executable discovery on macOS by deriving `--program-path` from `wmem_test` rather than `tshark`, whose bundle/output layout is not representative of all built test executables. |
| !4545 | Merged | Removes unnecessary Qt `Q_OBJECT` use and avoids `qobject_cast` where the factory already returns the concrete base type. Meta-object machinery should be retained only where its services are actually required. |
| !4544 | Merged | ISO15765 configuration no longer lets a missing/disabled LIN path short-circuit independent CAN registration, and CAN IDs are no longer incorrectly controlled by the LIN preference. |
| !4543 | Merged | TECMP creates child tvbuffs using the protocol-declared payload length rather than all captured bytes remaining in the parent. Child dissectors should see the semantic payload extent. |
| !4542 | Merged | Release-3.6 backport of the macOS Intel packaging CI job. Corroborates !4538/!4540; little independent design value. |
| !4541 | Merged | Normalizes user-facing spelling of TShark across documentation and Lua comments. Documentation consistency only. |
| !4540 | Merged | Stable/master-adjacent macOS Intel packaging CI addition. Corroborates !4538; no distinct durable rule. |
| !4539 | Merged | Stable backport of removing unnecessary `Q_OBJECT` declarations. Corroborates !4524. |
| !4538 | Merged | Gerald Combs adds a complete Intel macOS CI packaging path including build, tests, package prep, DMG creation, notarization, digesting, and publication hooks. |
| !4537 | Merged | John Thacker uses `conversation_set_dissector_from_frame_number()` for BT-DHT/uTP so historical frame dispatch is stable even though a shared UDP conversation switches protocol ownership over time and packets are revisited out of order in the GUI. |
| !4536 | Merged | Jaap Keuter moves included headers outside `extern "C"` blocks after GCC 10.3 reports templates/declarations with C linkage. Only declarations that actually require C linkage belong inside such blocks. |
| !4535 | Merged | Corrects Text2pcap naming in Qt help actions and translations. User-facing naming consistency only. |
| !4534 | Merged | Corrects Rawshark spelling/capitalization across Qt help UI and translations. User-facing naming consistency only. |
| !4533 | Merged | John Thacker strengthens BT-uTP recognition with protocol-version and receive-window constraints, disables obsolete v0 by preference, and explicitly holds off on default-enabling the heuristic until more testing exists. |
| !4532 | Merged | Gerald Combs teaches the HTML-to-text release-note converter to preserve semantic code/menu markup using backticks and quotes instead of flattening it. |
| !4531 | Merged | First implementation of a weekly CI job that edits generated registry/translation material and creates an MR. It had known API uncertainty and was promptly reverted by !4555, so treat it as superseded design history. |
| !4530 | Merged | A new Allied Telesis dissector adds an `llc.control` dissector table and preserves raw-data fallback rather than hard-coding a one-off child call into LLC. Alexis La Goutte also asks the contributor to force-squash the branch and notes AUTHORS is updated automatically. |
| !4529 | Merged | João Valverde makes numeric-field comparison against a quoted string consult that field's value-string table before issuing a type error, with regression tests for equality and set membership. |
| !4528 | Merged | Display-filter range integer conversion moves into the parser and the redundant integer AST node disappears. Later already-reviewed parser work provides stronger current guidance. |
| !4527 | Merged | Release-3.6 backport of the octal character-escape parser fix from !4519. |
| !4526 | Merged | Release-3.4 backport of the octal character-escape parser fix from !4519. |
| !4525 | Merged | RDPUDP AckVec support follows the then-current Microsoft specification. Alexis La Goutte asks for an example decode and the author supplies concrete packet-tree output, useful review evidence for a complex bit/RLE decoder. |
| !4524 | Merged | Gerald Combs removes `Q_OBJECT` from QObject-derived classes that do not use signals, slots, translation, or other meta-object services, avoiding unnecessary generated/compiled MOC code. |
| !4523 | Merged | Development version rolls from 3.5.1 to 3.7.0 across version metadata and docs. Release maintenance only. |
| !4522 | Merged | Initializes release-3.6 by updating translation resources, dependency bundle names, Debian sonames/packages, versioning, and branch-specific configuration together. |
| !4521 | Merged | Gerald Combs reduces fuzz passes/runtime on the older branch so finite CI/fuzz capacity can be concentrated on newer branches. Operational policy rather than a code convention. |
| !4520 | Merged | Normalizes field-mask widths and fixes several mask/abbreviation typos. Useful reminder to check mask width against the registered field type, but later typed-item tooling gives stronger validation guidance. |
| !4519 | Merged | João Valverde fixes octal character escapes with one to three digits; the parser had double-advanced its pointer on short escapes. The master fix is backed by stable !4526/!4527. |
| !4518 | Merged | Evan Huus makes `tvb_ip6_to_str()` take an explicit allocator and updates callers to use `pinfo->pool` when available; this supersedes !4515's unsafe tree-derived allocator sites. |
| !4517 | Merged | Display-filter semantic checking switches failure handling toward exceptions and consolidates one-byte literal treatment in ftypes. Internal architecture change with no additional review discussion. |
| !4516 | Merged | Evan Huus makes `tvb_ip_to_str()` take an explicit allocation scope and converts callers primarily to `pinfo->pool`. |
| !4515 | Closed | Superseded IPv6 allocator conversion. Evan Huus explicitly warns that `PNODE_POOL(tree)` is unsafe when `tree` can be NULL; merged successor !4518 uses explicit packet context where available and safe fallback otherwise. |
| !4514 | Merged | Marks protocol-equality display-filter tests skipped with an explicit explanation that protocol fvalue length semantics are not yet correct, preferring a documented known limitation over a misleading passing expectation. |
| !4513 | Merged | Registers additional LTE-RRC message dissectors for direct use/selection while sharing the generated parser. Narrow protocol dispatch extension. |
| !4512 | Merged | John Thacker threads an allocator through LISP `get_addr_str()` and recursive helpers instead of hard-coding ambient packet scope, then supplies `pinfo->pool` at packet-aware call sites. |
| !4511 | Merged | In documentation review, Gerald Combs asks for semantic Asciidoctor menu/code markup and notes that both HTML and text release notes are consumed; the renderer should preserve that meaning. This directly motivates !4532. |
