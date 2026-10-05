# Conventions from Wireshark MRs !10313 through !10362

## Generated code

Guy Harris repeatedly required changes to generated dissectors to be reflected in the authoritative ASN.1 template or conformance input and then regenerated. The relevant sequence is !10330/!10353, !10341/!10351, and !10343/!10352, with stable-branch corroboration in !10354-!10357.

## File recognition

John Thacker's !10331 documents that short byte signatures can be too collision-prone to justify magic-number classification, so MPEG remains a heuristic wiretap reader. !10328 says stronger and faster heuristic readers should be tried before weaker or slower readers. Filename extensions are priority hints rather than identity guarantees.

## Field representation

!10359 and !10347 replace two-state integer lookup tables with FT_BOOLEAN fields and shared true/false strings. For a masked Boolean, the registered width should still match the encoded carrier.

## Global state changes

In closed !10345, John Thacker rejected a broad set of per-dissector timing adjustments in favor of redissection after the global time-shift operation. Use this as negative architecture evidence: when a global state change invalidates derived packet results, central invalidation and redissection can be cleaner than compensation in every consumer.

## Static checks

!10315 expands the typed-item checker so macro-defined masks can be validated and then fixes the concrete field-registration problems revealed by the stronger check.

## Sentinel values

Guy Harris's merged !10349 handles protocol ID -1 as the documented “no protocol” value before normal protocol lookup.

## Build portability

!10329 records John Thacker's correction that GNU-compatibility macros are not sufficient proof of the actual compiler family. !10318 independently supports using shared project version helpers instead of hand-written version comparisons.
