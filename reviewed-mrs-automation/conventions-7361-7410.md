# Conventions from !7361-!7410

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Durable lessons extracted from this batch:

- !7405: Keep metadata that exists only between a parent protocol and its child dissectors in that dispatch interface instead of expanding global per-packet state. Anders Broman directed GRE key metadata into the existing child-call context.
- !7362: Keep state at its true semantic scope. John Thacker directed HTTP compression metadata into per-message context rather than recovering it by searching the display tree or storing it for the whole conversation.
- !7382 and !7377: Put resolved versus unresolved column-value selection behind a shared accessor so GUI, export, search, command-line, and other consumers cannot diverge.
- !7372, !7370, and !7367: Guy Harris showed that malformed declared record lengths should not prevent useful partial dissection before the first actual out-of-bounds field access. Finalize presentation extent after parsing when appropriate.
- !7371 and !7369: Guy Harris showed that field registration should reflect the semantic value type exposed to filters and consumers; a three-octet numeric value is a 24-bit integer, not merely an arbitrary byte sequence.
- !7366: Generated integer-width selection must consider whether both endpoints fit the target representation, not only how wide the encoded range is. Guy Harris identified the missing negative-bound case and Pascal Quantin completed the accepted correction.
- !7374 and !7402: A tool-language migration must preserve behavior of every public mode and update every supported caller across build, packaging, documentation, and CI.
- !7397: Guy Harris replaced an instruction-set-specific CPU identification path with operating-system facilities, reinforcing architecture-neutral host diagnostics.
- !7401: Uli Heilmeier's review favors probing the needed package or capability over distribution-name branching when the setup script already supports tolerant availability checks. This MR was closed, so the lesson is review guidance rather than implementation precedent.
- !7409 and !7368: Use a topic branch for merge-request work, and distinguish an attached reproducer capture from a maintained repository regression fixture.

Merged work is weighted above closed or superseded submissions. See review-findings-7361-7410.md for the complete 50-MR inventory.
