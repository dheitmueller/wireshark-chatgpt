# Detailed durable conventions from Wireshark MRs !6511-!6560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Retained typed values

Merged !6560 shows that a display-filter reference can remain live after the protocol tree that supplied its value is gone. The accepted design duplicates retained values according to field type.

**Rule:** when typed data crosses a lifetime boundary, the retaining object must have a copy whose storage remains valid for that lifetime. Copy behavior must follow the type's representation rather than assuming every field value has identical ownership.

## Complete structured-field visibility

During merged !6559, Alexis La Goutte requested that reserved bits in a Geneve option word remain visible along with the currently defined bits.

**Rule:** where practical, account for the complete structured wire value, including reserved ranges. This exposes unexpected nonzero bits and keeps future specification changes observable.

## Generated-source authority

Merged !6547 fixes ASN.1 source material after generated dissectors and source checks diverged. Alexis explicitly directed the contributor to update the template or fix the generator.

**Rule:** make generated-code corrections in the authoritative template/configuration or generator path so regeneration reproduces them.

## Valid empty values

Merged !6525 and !6542 preserve protocol fields whose encoded values may validly have length zero, while avoiding presentation helpers that require nonempty input. !6535 carries the NetFlow correction to a maintained branch.

**Rule:** field presence and payload length are independent properties. Preserve a valid empty field and guard only the helper whose contract requires content.

## Recoverable protocol diagnostics

In merged !6522, John Thacker distinguished a specification violation that remains parseable from a condition that prevents reliable continued decoding.

**Rule:** report recoverable specification violations with protocol-oriented expert diagnostics and continue when trustworthy parsing is still possible. Reserve aborting/malformed behavior for conditions where decoding cannot proceed reliably.

## Unknown subtype fallback

Merged !6530 dispatches reassembled fragmented DPP data to its specialized decoder. Follow-up !6531 ensures a different Vendor Specific subtype is still shown as raw payload rather than disappearing.

**Rule:** specialized dispatch must preserve observability for unknown sibling subtypes. If an envelope is understood but its subtype is not, expose the remaining payload as raw/undecoded data.

## Dependency-major CI transitions

Merged !6516 moves primary 64-bit Windows builds to Qt 6. Review explicitly raised continuing Qt 5.15 coverage; merged !6534 adds a focused Windows Qt 5 UI build while other Qt 5 jobs remain.

**Rule:** when a new dependency major becomes primary, retain deliberate CI coverage for an older major that is still supported. A narrower compatibility job can be sufficient if it exercises the affected surface.

## Typed display-filter operators

Merged !6518 implements unary arithmetic across parsing, semantic type checks, field-type capabilities, execution, and tests.

**Rule:** a new display-filter operator is not complete at parser acceptance. Model the operator as a typed capability and keep syntax, semantics, execution, diagnostics, and regression tests synchronized.
