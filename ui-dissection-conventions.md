# Wireshark UI Dissection Conventions

This file records durable conventions for GUI-triggered dissection and derived UI work. Current upstream source remains authoritative.

## Do not perform derived dissections while the packet-list model is intentionally unavailable or frozen

Derived GUI work such as packet coloring, minimaps, or scrollbar summaries should respect the packet list's lifecycle. If the packet-list model is intentionally unavailable while profiles or preferences are being changed, running extra dissections is both wasted work and potentially semantically wrong: the dissections may observe the temporary/default preference state and their results will be invalidated by the required redissection anyway.

Merged MR !23191, authored and merged by John Thacker, stops scroll-minimap dissection while the packet list is frozen. The MR notes both reasons: the scrollbar is not being displayed while the model is absent, and profile changes can temporarily leave preferences between reset and application of the new values.

**Implementation rule:** background/idle GUI features that derive data by dissecting packets must observe the same freeze/rebuild lifecycle as the primary packet model. Suppress derived work while the model is frozen or configuration is in transition, then let the normal redissection/rebuild path repopulate the derived state after the new configuration is stable.

**Confidence:** Very high. Merged master performance/correctness fix authored and merged by John Thacker, with a concrete profile-change failure mode.
