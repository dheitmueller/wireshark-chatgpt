# Wireshark Protocol-Tree Analysis Conventions

This file records conventions for keeping semantic dissection behavior independent from optional protocol-tree presentation work. Current upstream code remains authoritative.

## Do not gate semantic diagnostics on tree demand

Whether a protocol tree is being built or a protocol is referenced by a filter is a presentation/performance concern. Expert information and other semantic analysis can have independent consumers.

Merged MR !8023, authored by Guy Harris, fixes the Frame dissector so the “reported length less than captured length” expert diagnostic is still generated when `proto_field_is_referenced()` says Frame tree items are not needed. Guy's accepted code comments that expert information must still appear in consumers such as the Expert Information dialog and describes the duplicated optimization path as fragile.

**Implementation rule:** do not make correctness checks, expert diagnostics, state learning, or other semantically observable analysis conditional on whether tree fields are requested. Limit tree-demand checks to work whose only observable effect is tree construction or presentation.

**Review rule:** when adding a fast path for an unreferenced protocol or a null tree, compare the semantic side effects of both paths. If diagnostics or state are produced only in the full-tree branch, the optimization changes behavior rather than merely saving work.

**Confidence:** Extremely high. Merged fix authored by Guy Harris with the semantic/presentation distinction stated directly in accepted code.
