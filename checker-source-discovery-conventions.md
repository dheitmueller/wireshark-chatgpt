# Checker Source Discovery Conventions
Merged MRs 6006 and 5978 improve check_typed_item_calls.py. The checker distinguishes registered fields from local declarations and externally looked-up IDs, expands intended dissector coverage, and anchors source matching at .c so .c.swp and .c~ editor artifacts are not parsed as C.

Rule: static repository checkers should model their semantic source domain explicitly. File discovery must match real source files, and symbol checks must distinguish declaration, definition, and legitimate external resolution before reporting absence.

Confidence: very high; merged Martin Mathieson and John Thacker tooling changes.
