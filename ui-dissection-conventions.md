# Wireshark UI Dissection Conventions

This file records durable conventions for GUI-triggered dissection and derived UI work. Current upstream source remains authoritative.

## Do not perform derived dissections while the packet-list model is intentionally unavailable or frozen

Derived GUI work such as packet coloring, minimaps, or scrollbar summaries should respect the packet list's lifecycle. If the packet-list model is intentionally unavailable while profiles or preferences are being changed, running extra dissections is both wasted work and potentially semantically wrong: the dissections may observe the temporary/default preference state and their results will be invalidated by the required redissection anyway.

Merged MR !23191, authored and merged by John Thacker, stops scroll-minimap dissection while the packet list is frozen. The MR notes both reasons: the scrollbar is not being displayed while the model is absent, and profile changes can temporarily leave preferences between reset and application of the new values.

**Implementation rule:** background/idle GUI features that derive data by dissecting packets must observe the same freeze/rebuild lifecycle as the primary packet model. Suppress derived work while the model is frozen or configuration is in transition, then let the normal redissection/rebuild path repopulate the derived state after the new configuration is stable.

**Confidence:** Very high. Merged master performance/correctness fix authored and merged by John Thacker, with a concrete profile-change failure mode.

## Drive syntax-aware editor assistance from parser semantics, not widget heuristics

Autocomplete and other syntax-aware UI should ask the parser/compiler what is grammatically legal at the current position instead of inferring legality from generic success/failure state or from widget-local heuristics. Parser error categories can be part of that semantic contract: an unexpected end of expression may mean that a field is legal next, while a complete valid expression requires an operator, and a more specific grammar state may require a different token class entirely.

Merged MR !23146, authored and approved by John Thacker, fixes `DisplayFilterEdit` so field completion follows exactly those cases: an empty preamble permits a field; a failed compile with `DF_ERROR_UNEXPECTED_END` permits a field; other parse failures do not; and a successful non-empty filter does not permit another field because an operator must come first. Merged companion MR !23147 adds a distinct scanner error for end-of-input after the layer operator (`#`) and regression tests for `ip.dst#` and `ip.dst#ip`, preventing that grammar state from being misclassified as one where field completion is appropriate.

**Implementation rule:** treat parser state and sufficiently precise diagnostics as the source of truth for syntax-aware UI. If editor behavior depends on distinguishing grammar states, give those states distinct parser errors and regression tests; do not reduce all parse failures to a single boolean and reconstruct grammar rules in the widget.

**Confidence:** Very high. Two merged master changes authored and approved by John Thacker, with parser regression tests directly covering the UI-relevant distinction.
