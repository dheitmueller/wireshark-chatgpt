# Durable synthesis — !1961–!2010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This batch contains 47 merged MRs and three closed/unmerged MRs (!1977, !1968, !1963). Closed submissions are retained only as lower-weight historical context.

## Wiretap registration

Merged Guy Harris MR !1967 establishes the foundation for later Wiretap runtime registration. CMake supplies the actual Wiretap source set to `make-regs.py`; the generator discovers module-local `register_*` routines, creates the callback table, and `wtap_init()` invokes it. Central infrastructure should orchestrate registration while each format module owns its registration entry point. Generated-registry dependencies must track the source files being scanned.

## Optional build boundaries

Merged Guy Harris MR !1965 removes `HAVE_PLUGINS` guards from pcapng handler infrastructure because built-in handlers also need the same mechanism. A feature guard should cover the genuinely optional surface, such as loading external plugins, not shared dispatch machinery used by built-in code.

## Writer architecture and validation

Merged Guy Harris MR !1969 consolidates btsnoop output into one writer whose behavior depends on the writer encapsulation and associated pseudo-header semantics. It fixes H1/H4 record generation and validates the result by converting several captures through `editcap -F btsnoop` and checking byte identity. For representation-preserving writer changes, round-trip or byte-identity testing is stronger than merely confirming that the result can be reopened.

## Capture diagnostics

Guy Harris's merged !1995, !1996, !1997, !2008, !2009, and release backport !2010 evolve dumpcap errors toward preserving the underlying failure, naming the affected interface, providing actionable secondary guidance, and classifying Windows error 1617 without depending on the localized sentence inside the error text. Stable native structure or error codes should drive classification; human-readable platform text is for presentation.

## Statistics lifecycle

Merged !1983–!2000 repeatedly separate fixed table schema from mutable statistics state. Reinitialization first looks for an existing stat table and resets it through the registered reset callback; only a missing table is created and populated with fixed rows. Reopening or retapping should reset counters rather than duplicate immutable rows or table objects.

## Presentation defaults

In merged !1973, Pascal Quantin asks that a long-standing LTE-RRC tree-layout change be optional and off by default, while Anders Broman describes the usability argument for the new layout. The accepted result preserves the established default and exposes the alternative. Merged !1974 separately aligns LTE and 5GS behavior where the decoded NAS payload itself should be presented consistently. Distinguish contested presentation preference from a semantic consistency fix.

## Submission scope

In merged !1975, Alexis La Goutte asks that an unrelated MsQuic change be moved to a separate MR; the contributor removes it from the version-negotiation work. Keep protocol-feature MRs focused and move unrelated cleanup or behavior changes into separate submissions.

No SMPTE ST 291/VANC packet type was encountered.
