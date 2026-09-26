# Durable conventions from Wireshark MRs !8011-!8060

Corpus snapshot: `ddcaa22b51c68f594e425a23388c3a2086813054`.

## Qt object lifetime and nested event loops

Merged master !8038 (John Thacker), with review from Guy Harris and Tomasz Moń, shows that `deleteLater()` is not a guarantee that an object survives until the currently executing slot returns if Wireshark enters a nested event loop. `MainApplication::processEvents()` can process a pending DeferredDelete while the original call stack is still active. The accepted `filesSelected` connection avoids the observed Qt 6.3 crash by running earlier, but the review explicitly treats that ordering as tactical rather than a permanent lifetime guarantee. Prefer designs that avoid unnecessary nested event loops; when re-entry is unavoidable, make ownership and deferred execution boundaries explicit.

## Sibling PDUs must not inherit transient conversation context

Merged master !8013 (John Thacker) fixes PPP raw-HDLC streams containing multiple independent frames in one enclosing byte stream. The next sibling PDU must not inherit the previous sibling's “most recent conversation” context. Merged !8018 tried a generic reset around every dissector call, but its own follow-up discussion records regressions and John's recommendation to revert that broad approach in favor of resetting state at semantic multi-PDU boundaries. Preserve parent context for true nested encapsulation; reset transient context when the carrier knows it is dispatching a new sibling PDU.

## Tree-demand optimizations must not suppress semantic diagnostics

Merged !8023, authored by Guy Harris, makes Frame expert information appear even when the Frame protocol itself is not referenced by a filter and tree-item generation is optimized away. Whether a protocol tree is needed is a presentation/performance decision; expert information and other semantic analysis can have independent consumers. Do not make correctness or diagnostics conditional on `proto_field_is_referenced()` merely to avoid constructing tree items.

## Registered dissector names are global registry keys

Merged master !8043 converts many anonymous handles to `register_dissector()`. During review, Anders Broman caught a startup assertion caused by duplicate registered names. Treat a registered dissector name as a globally unique programmatic key, not a local label. Broad registration migrations should include an initialization/startup run such as `tshark -v` so duplicate-name assertions are exercised, not just a successful compile.

## One filter abbreviation cannot describe incompatible registered field types

Merged master !8050 includes Pascal Quantin's explicit review that the same filter name cannot be shared between `FT_INT8` and `FT_UINT64` registrations. When two encodings of a concept require incompatible Wireshark field types, either normalize the type where that is semantically correct or use distinct abbreviations. Filter usability does not override field-registry type consistency.

## Decode packet text before storing it as an FT_STRING value

Merged master !8012 (John Thacker) decodes percent-decoded form-urlencoded data as UTF-8, substituting replacement characters as required, before passing it to `proto_tree_add_string()`. The MR specifically notes that otherwise invalid text can produce invalid JSON/XML exports. The semantic field value must satisfy Wireshark's UTF-8 text contract; presentation helpers such as `format_text()` remain for labels rather than replacing the stored value.

## Prefer authoritative transaction state over redundant handoff fields

Merged master !8024, authored by Guy Harris, removes a duplicated AppleTalk/DSI command field from the handoff structure and returns the transaction record that already owns the command. Avoid copying authoritative transaction/session state into parallel context fields when callers can use the owning object directly; duplicated state widens synchronization and lifetime obligations.

## Corroborating findings

- !8022 (Guy Harris, with !8017 backport) diagnoses and repairs the impossible TVBuff condition where reported length is less than captured length. Later notebook evidence already records the stronger captured/reported-length invariant, so this run treats !8022 as early high-authority corroboration.
- !8049 (John Thacker) fixes UTF-8 label truncation using the one-past-end contract of `g_utf8_prev_char()`; later text-boundary rules already cover encoding-safe truncation.
- !8030-!8032 are stable-branch fixes for an F5 trailer heuristic infinite loop and corroborate the existing parser-progress/no-progress rule.
- !8053 records review catching an unrelated dependency-upgrade commit in a protocol MR; the author removed it before merge. Keep topic branches and MR scope focused.
- Closed !8040 is useful only as submission-process evidence: Alexis La Goutte requested a rebase, a representative pcap, and use of a topic branch rather than the contributor's protected master. The work continued in a later accepted MR, so this draft is not implementation authority.
- Closed !8041 was superseded by later dependency work and is not treated as accepted design guidance.
