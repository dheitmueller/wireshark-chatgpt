# Checker Source Discovery Conventions
Merged MRs 6006 and 5978 improve check_typed_item_calls.py. The checker distinguishes registered fields from local declarations and externally looked-up IDs, expands intended dissector coverage, and anchors source matching at .c so .c.swp and .c~ editor artifacts are not parsed as C.

Rule: static repository checkers should model their semantic source domain explicitly. File discovery must match real source files, and symbol checks must distinguish declaration, definition, and legitimate external resolution before reporting absence.

Confidence: very high; merged Martin Mathieson and John Thacker tooling changes.


## Exclude generated templates by source class

Merged MR 5663 changed comments in ASN.1 packet templates but triggered a false positive from validate-clang-check.sh because those templates are not standalone final dissectors. John Thacker identified the mismatch. Jaap Keuter explicitly recommended generalizing the checker to skip all packet template C files instead of keeping a protocol-specific exception.

**Discovery rule:** if a checker is defined for compiled or final C sources, template inputs are outside its semantic domain even when their filenames end in .c. Express that exclusion as a general source-class rule instead of accumulating one-off protocol exceptions.

**Confidence:** Very high. Direct maintainer guidance from Jaap Keuter, corroborated by John Thacker's diagnosis, on a merged MR.
