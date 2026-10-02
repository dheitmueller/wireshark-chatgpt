# Wireshark Protocol-Tree Analysis Conventions

This file records conventions for keeping semantic dissection behavior independent from optional protocol-tree presentation work. Current upstream code remains authoritative.

## Do not gate semantic diagnostics on tree demand

Whether a protocol tree is being built or a protocol is referenced by a filter is a presentation/performance concern. Expert information and other semantic analysis can have independent consumers.

Merged MR !8023, authored by Guy Harris, fixes the Frame dissector so the “reported length less than captured length” expert diagnostic is still generated when `proto_field_is_referenced()` says Frame tree items are not needed. Guy's accepted code comments that expert information must still appear in consumers such as the Expert Information dialog and describes the duplicated optimization path as fragile.

**Implementation rule:** do not make correctness checks, expert diagnostics, state learning, or other semantically observable analysis conditional on whether tree fields are requested. Limit tree-demand checks to work whose only observable effect is tree construction or presentation.

**Review rule:** when adding a fast path for an unreferenced protocol or a null tree, compare the semantic side effects of both paths. If diagnostics or state are produced only in the full-tree branch, the optimization changes behavior rather than merely saving work.

**Confidence:** Extremely high. Merged fix authored by Guy Harris with the semantic/presentation distinction stated directly in accepted code.

## A tree item needed by expert analysis must exist even when no visible tree was requested

Merged !2103 fixes IPv4 TTL expert information that disappeared when no protocol tree was requested. The TTL item had been created only inside `if (tree)`, yet later expert logic attached diagnostics to that item. The accepted change creates the item through the normal proto-tree API regardless; Pascal Quantin explicitly asked the contributor to audit other expert inputs for the same tree-gating pattern.

**Implementation rule:** if later semantic logic, expert information, generated metadata, or another non-presentation consumer needs a proto item, do not initialize that item only in a visible-tree branch. Wireshark's tree APIs support null/no-display tree operation without changing semantic behavior.

**Review rule:** when removing one `if (tree)` correctness bug, inspect nearby fields used by expert/state logic for the same pattern rather than assuming it is isolated.

**Confidence:** Very high. Merged master fix with direct Pascal Quantin review; independently corroborates the Guy Harris-authored !8023 rule.
