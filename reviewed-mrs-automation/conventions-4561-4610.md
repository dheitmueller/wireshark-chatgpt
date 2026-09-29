# Durable conventions extracted from MR review 4561–4610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

Merged work receives primary weight. Stable backports corroborate master behavior. Closed MRs are used only where they document a rejected/superseded design or submission lesson.

## Reassembly copies are bounded by destination capacity, not just source availability

Merged master !4603, authored by Gerald Combs, computes the remaining HCI ISO reassembly-buffer capacity before copying a fragment. If captured fragment bytes exceed that capacity, the dissector reports malformed length and copies only what fits. Stable !4607 and !4608 corroborate the fix.

Merged master !4604 applies the same defensive idea to Bluetooth SDP continuation state: the packet-declared state length is checked against the protocol's fixed maximum before the value is retained in longer-lived state.

**Rule:** source bounds and destination capacity are independent invariants. Before copying packet-derived data into fixed-size or pre-sized reassembly state, prove that the copy fits the destination; report malformed excess and only recover within an explicitly safe bound.

## Stateful reassembly needs both stable identity and retained first-fragment semantics

Merged master !4599 assigns WebSocket fragment series a stable per-conversation reassembly ID and retains the opcode from the first fragment until the complete reassembled payload exists. The earlier closed !4583 was abandoned specifically because fragment identity had not been solved correctly.

**Rule:** a fragmented-message key must identify one logical message throughout its lifetime, and metadata carried only by the first fragment must be retained alongside reassembly state when it controls how the completed payload is dispatched.

## Conversation identity must follow the protocol's identifiers

Merged master !4592, authored by John Thacker, models uTP endpoints with uTP connection IDs rather than relying only on UDP endpoints. It accounts for the protocol's paired direction-dependent IDs, SYN semantics, partial observation, and wildcard completion, then exposes one generated stream number for the logical conversation.

**Rule:** if a transport-like protocol defines its own endpoint/session identifiers, use those identifiers—and the protocol's direction/handshake rules—to form conversation state. A lower-layer tuple alone can merge or split sessions incorrectly.

## Sentinels are not ordinary sequence numbers

Merged master !4576, authored by John Thacker, prevents placeholder ACK value zero from being passed through wrap-aware TCP sequence comparison by adding an explicit validity boolean. Stable !4582 corroborates the fix.

**Rule:** when a numeric value has a sentinel meaning such as "unknown" or "not present", carry validity separately before applying modular sequence arithmetic or ordinary protocol comparisons. A sentinel that happens to be representable in the numeric domain is not a real protocol value.

## Enforce parser progress after each packet-controlled iteration

Merged master !4570, authored by Gerald Combs, records the start offset for each BT-DHT bencoded-list element and terminates with expert information whenever the child parse returns an offset that did not advance. Stable !4587 corroborates it.

**Rule:** repeated parsers should enforce the monotonic-progress invariant at the loop boundary, not merely trust every child helper to use one particular failure sentinel. After a recoverable iteration, the next offset must be greater than the starting offset or the loop must terminate.

## Formatting macros must match the actual typedef and varargs stack

Merged master !4564 fixes macOS format diagnostics caused by using C99 PRI macros with GLib integer typedefs whose underlying C type can differ by platform. Pascal Quantin pointed to Wireshark's GLib formatting guidance, while João Valverde highlighted that the correct family also depends on the formatting function path: GLib formatting with GLib types and standard printf-family formatting with C99 types must mesh end to end.

**Rule:** choose 64-bit format macros from the actual argument type and the varargs formatting API, not from bit width alone. Cross-platform builds are required because `uint64_t` and `guint64` may have equal width but different underlying C types.

## Compatibility shims must not steal another library's exported symbol namespace

Merged master !4569 moves Wireshark's pre-GLib-2.68 `g_memdup2` compatibility implementation to a static-inline header helper. The MR explicitly notes that a shared library should not export a symbol that belongs to another shared library; stable !4579 corroborates the change.

**Rule:** a compatibility implementation that mimics a dependency API should remain local unless Wireshark intentionally owns that public symbol. Avoid exporting a duplicate dependency symbol that can collide at dynamic-link time.

## Registered filter names cannot collide with language keywords

Merged master !4567 extends protocol filter-name validation to reject display-filter reserved words.

**Rule:** registry identifiers that feed a parser must satisfy both character syntax and lexical namespace constraints. Reject reserved keywords at registration time so a valid registration can never create ambiguous or impossible filter syntax.

## Split independent fixes out of a large feature MR

During merged !4577, Alexis La Goutte asked that existing TCPCL fixes be separated from the TCPCLv4 feature; the author moved the fixes into another MR before the feature merged. Closed !4581 separately corroborates the project's preference for clean topic branches rather than submitting from a long-lived master branch.

**Submission rule:** keep a large feature MR focused on the feature. Move unrelated correctness fixes to independent submissions when they can stand on their own, and use a dedicated topic branch so review history and scope stay clear.

## Treat authored maintainer changes as evidence even without review comments

There was no substantive Guy Harris discussion comment in this batch. However merged master !4571 and its stable backport !4574 were authored by Guy and remove unused protocol-ID lookups from AUTOSAR NM. They are high-authority evidence for that narrow cleanup: do not create registry coupling or perform lookups whose result has no semantic use. This is intentionally not generalized into a broader API rule.
