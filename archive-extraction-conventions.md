# Wireshark Archive Extraction Conventions

This file records durable security conventions for extracting or importing archive contents. Current upstream implementation remains authoritative.

## Treat archive entry paths as hostile and keep extraction inside the selected root

Archive member names are input data, not trusted filesystem paths. Before creating or overwriting a destination, ensure that path components supplied by the archive cannot traverse outside the directory the user selected for extraction or import. Reject unsafe entries rather than allowing `..`, absolute paths, or equivalent path manipulation to redirect writes elsewhere.

Merged MR !24035 fixes a traversal vulnerability in Qt ZIP loading/import code and was immediately identified for backport to both the 4.4 and 4.6 stable branches; merged !24041 and !24042 carry those stable fixes. The stable propagation gives unusually strong evidence that containment of archive extraction paths is a security contract rather than presentation-only validation.

**Implementation rule:** resolve each archive member relative to an explicit extraction root and validate containment before writing. The archive controls the relative member name only; it must never be able to choose an arbitrary filesystem destination.

**Confidence:** Very high. Merged master security fix with accepted backports to both supported stable branches.
